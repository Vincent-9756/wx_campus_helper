<template>
	<view class="content">
		<view class="content_item">
			<view class="item_top">
				<view class="taskType">
					任务名：{{name}}
				</view>
				<view class="taskType" style="margin-top: 0;">
					佣金：{{price}}
				</view>
			</view>
			<view class="item_bottom" @tap="toCall">
				<view class="accpetTask">
					联系电话：{{phone}}
				</view>
			</view>
		</view>
		<view class="taskDetail">
			<view class="detailItem">
				发布人：{{creatorName}}
			</view>
			<view class="detailItem">
				发布时间：{{createTime}}
			</view>
			<view class="detailItem">
				接受人：{{acceptorName}}
			</view>
			<view class="detailItem">
				接受人联系方式：{{acceptorPhone}}
			</view>
			<view class="detailItem">
				接受人专业：{{majorName}}
			</view>
			<view class="detailItem">
				订单编号：{{code}}
			</view>
			<view class="detailItem">
				地址：{{address}}
			</view>
			<view class="detailItem">
				任务内容：{{content}}
			</view>
		</view>
		
		<view class="btnBox">
			<view class="cancelOrder" @tap="cancelTask" v-if="status != '已取消'">
				取消订单
			</view>
			<view class="doneOrder" @click="finishOrder">
				完成订单
			</view>
		</view>
		
		<view class="acceptTask" :class="{'doneColor':done,'doingColor':doing}">
			{{status}}
		</view>
	</view>
</template>

<script>
	import {URL} from '@/common/util.js'
	export default {
		data() {
			return {
				creatorName: '',
				createTime: '',
				acceptorPhone: '',
				acceptorName: '',
				majorName: '',
				name: '',
				phone: '',
				price: '',
				code: '',
				content: '',
				address: '',
				orderId: '',
				status: '',
				done: false,
				doing: false
			}
		},
		onLoad(e) {
			// 调取接口获取订单详细信息
			uni.request({
				url: URL + '/task/queryTask',
				method: 'POST',
				data: {
					creatorId: uni.getStorageSync('userId'),
					id: e.id
				},
				success: res => {
					console.log(res)
					if((res.data.data[0].finishAccept == "CONFIRM" || res.data.data[0].finishCreator == "CONFIRM") && res.data.data[0].status != "FINISHED") {
						this.status = '待确认'
						this.doing = true
					}
					else if(res.data.data[0].status == "DOING") {
						this.status = '进行中'
						this.doing = true
					}
					else if(res.data.data[0].status == "FINISHED") {
						this.status = '已完成'
						this.done = true
					}
					else if(res.data.data[0].status == "PUBLIC") {
						this.status = '待接单'
						this.doing = true
					}
					else if(res.data.data[0].status == "CANCELED") {
						this.status = '已取消'
						this.done = true
					}
					this.creatorName = res.data.data[0].creatorName
					this.createTime = res.data.data[0].createTime
					this.acceptorName = res.data.data[0].acceptorName || '无'
					this.acceptorPhone = res.data.data[0].acceptorPhone || '无'
					this.majorName = res.data.data[0].majorName || '无'
					this.name = res.data.data[0].name
					this.phone = res.data.data[0].creatorPhone
					this.price = res.data.data[0].reward
					this.code = res.data.data[0].code
					this.content = res.data.data[0].content
					this.address = res.data.data[0].termini
					this.orderId = res.data.data[0].id
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			toCall() {
				uni.makePhoneCall({
				    phoneNumber: this.phone
				});
			},
			// 点击完成订单函数
			finishOrder() {
				uni.request({
					url: URL + '/task/finishTask',
					method: 'POST',
					data: {
						creatorId: uni.getStorageSync('userId'),
						id: this.orderId
					},
					success: res => {
						console.log(res)
						if(res.data.result == "SUCCESS") {
							uni.showToast({
								icon: 'success',
								title: res.data.msg,
								duration: 2000
							})
							setTimeout(() => {
								uni.reLaunch({
									url: '/pages/index/index'
								})
							}, 2000)
						} else {
							uni.showToast({
								icon: 'loading',
								title: res.data.msg,
								duration: 2000
							})
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 点击取消订单函数
			cancelTask() {
				uni.request({
					url: URL + '/task/cancelTask',
					method: 'POST',
					data: {
						creatorId: uni.getStorageSync('userId'),
						id: this.orderId
					},
					success: res => {
						console.log(res)
						if(res.data.result == "SUCCESS") {
							uni.showToast({
								icon: 'success',
								title: res.data.msg,
								duration: 2000
							})
							setTimeout(() => {
								uni.reLaunch({
									url: '/pages/index/index'
								})
							}, 2000)
						} else {
							uni.showToast({
								icon: 'loading',
								title: res.data.msg,
								duration: 2000
							})
						}
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	page {
		width: 100%;
		height: 100%;
		display: flex;
	}
	
	.content {
		width: 750rpx;
		flex: 1;
		display: flex;
		flex-direction: column;
		background-color: #FFFFFF;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		height: 100%;
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
	
	.taskType {
		height: 70rpx;
		line-height: 70rpx;
		text-align: center;
		font-size: 22px;
		margin-top: 50rpx;
		color: #fed600;
	}
	
	.item_bottom {
		width: 700rpx;
		height: 60rpx;
		line-height: 60rpx;
		text-align: center;
		color: #fa6f6f;
		position: absolute;
		right: 5rpx;
		bottom: 20rpx;
		font-size: 22px;
	}
	
	.taskDetail {
		width: 750rpx;
		padding: 25rpx;
		display: flex;
		flex-direction: column;
		color: #747474;
		font-size: 16px;
	}
	
	.detailItem {
		width: 700rpx;
		line-height: 70rpx;
		text-align: center;
		word-wrap: break-word;
		word-break: break-all;
		overflow: hidden;
	}
	
	.acceptTask {
		width: 300rpx;
		height: 100rpx;
		color: #FFFFFF;
		text-align: center;
		line-height: 100rpx;
		position: absolute;
		left: 50%;
		bottom: 30rpx;
		transform: translate(-50%);
	}
	
	.doneColor {
		color: #4CD964;
	}
	
	.doingColor {
		color: #ff0000;
	}
	
	.btnBox {
		width: 700rpx;
		height: 150rpx;
		margin: 0 auto;
		margin-top: 20rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.cancelOrder,
	.doneOrder {
		width: 200rpx;
		height: 100rpx;
		text-align: center;
		line-height: 100rpx;
		color: #FFFFFF;
		border-radius: 10px;
	}
	
	.cancelOrder {
		background: #F5C253;
	}
	
	.doneOrder {
		background: #4CD964;
		margin-left: 20rpx;
	}
</style>
