<template>
	<view class="content">
		<view class="content_item" @tap="toOpenDetail(e.orderId)" v-for="e in orderArr" :key="e.orderId">
			<view class="item_top">
				<view class="taskType">
					任务：{{e.name}}
				</view>
				<view class="taskAddress">
					<view class="addressText">
						地址：{{e.address}}
					</view>
					<image src="/static/images/addr.png" mode="aspectFit"></image>
				</view>
				<view class="taskPrice">
					佣金：{{e.price}}
				</view>
			</view>
			<view class="item_bottom">
				<view class="accpetTask" :class="{'doneColor':e.done,'doingColor':e.doing}">
					{{e.status}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {URL} from '@/common/util.js'
	export default {
		data() {
			return {
				orderArr: []
			}
		},
		onLoad() {
			if(!uni.getStorageSync('userInfo')) {
				uni.showToast({
					icon: 'none',
					title: '请先前往登录'
				})
				return false
			}
			// 获取我接受的订单列表
			this.orderArr.splice(0)
			uni.request({
				url: URL + '/task/queryTask',
				method: 'POST',
				data: {
					acceptorId: uni.getStorageSync('userId')
				},
				success: res => {
					console.log(res)
					let done = false
					let doing = false
					res.data.data.forEach(e => {
						if((e.finishAccept == "CONFIRM" || e.finishCreator == "CONFIRM") && e.status != "FINISHED") {
							e.status = '待确认'
							doing = true
						}
						else if(e.status == "DOING") {
							e.status = '进行中'
							doing = true
						}
						else if(e.status == "FINISHED") {
							e.status = '已完成'
							done = true
						}
						else if(e.status == "CANCELED") {
							e.status = '已取消'
							done = true
						}
						this.orderArr.push({
							name: e.name,
							price: e.reward,
							address: e.termini,
							status: e.status,
							orderId: e.id,
							done: done,
							doing: doing
						})
					})
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			// 前往某一订单详情页面
			toOpenDetail(e) {
				uni.navigateTo({
					url: `/pages/order/acceptOrderDetail?id=${e}`
				})
			}
		}
	}
</script>

<style>
	.content {
		width: 750rpx;
		/* flex: 1; */
		display: flex;
		flex-direction: column;
		background-color: #F1F1F1;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		/* height: 100%; */
		/* #endif */
		position: relative;
	}
	
	.content_item {
		width: 700rpx;
		height: 300rpx;
		border-radius: 10px;
		background-color:#FFFFFF;
		box-shadow: 2px 2px 5px #333333;
		color: #747474;
		position: relative;
		margin: 25rpx 25rpx 0 25rpx;
	}
	
	.content_item:last-child {
		margin-bottom: 50rpx;
	}
	
	.item_top {
		width: 700rpx;
		height: 230rpx;
		display: flex;
		flex-direction: column;
		padding-left: 20rpx;
	}
	
	.taskType,
	.taskAddress,
	.taskPrice {
		width: 700rpx;
		height: 70rpx;
		line-height: 70rpx;
	}
	
	.taskAddress {
		display: flex;
		align-items: center;
	}
	
	.taskAddress>image {
		width: 50rpx;
		height: 50rpx;
	}
	
	.item_bottom {
		width: 200rpx;
		height: 60rpx;
		line-height: 60rpx;
		text-align: center;
		color: #fa6f6f;
		position: absolute;
		right: 5rpx;
		bottom: 5rpx;
	}
	
	.doneColor {
		color: #4CD964;
	}
	
	.doingColor {
		color: #ff0000;
	}
</style>
