<template>
	<view class="content">
		<view class="content_item" v-for="i in taskArr" :key="i.taskId">
			<view class="item_top">
				<view class="taskType">
					任务：{{i.taskName}}
				</view>
				<view class="taskAddress">
					<view class="addressText">
						地址：{{i.address}}
					</view>
					<image src="/static/images/addr.png" mode="aspectFit"></image>
				</view>
				<view class="taskPrice">
					佣金：{{i.price}}
				</view>
			</view>
			<view class="item_bottom" @tap="toAccept(i.taskId)">
				<view class="accpetTask">
					查看详情
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
				taskArr: []
			}
		},
		onShow() {
			// #ifdef MP-WEIXIN
			if(wx.hideHomeButton){  
				wx.hideHomeButton();  
			}  
			// #endif
		},
		onLoad(e) {
			this.taskArr.splice(0)
			const $this = this
			uni.request({
				url: URL + '/task/queryPublicTaskList',
				method: 'POST',
				data: {
					id: uni.getStorageSync('userId'),
					type: e.type
				},
				success: res => {
					console.log(res)
					res.data.data.forEach(e=> {
						console.log(e)
						$this.taskArr.push({
							taskName: e.name,
							price: e.reward,
							address: e.termini,
							taskId: e.id
						})
					})
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			toAccept(e) {
				uni.navigateTo({
					url: `/pages/helpOrder/helpOrderDetail?id=${e}`
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
		margin-left: 10px;
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
</style>
