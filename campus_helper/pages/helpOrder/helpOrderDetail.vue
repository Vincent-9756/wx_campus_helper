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
				订单编号：{{code}}
			</view>
			<view class="detailItem">
				地址：{{address}}
			</view>
			<view class="detailItem">
				任务内容：{{content}}
			</view>
		</view>
		<view class="acceptTask" @tap="acceptOrder">
			接单
		</view>
		<uni-popup ref="showtip" type="center" :mask-click="false">
			<view class="uni-tip">
				<text class="uni-tip-title">前往认证</text>
				<view class="uni-tip-content">
					当前用户尚未认证，请前往认证
				</view>
				<view class="uni-tip-group-button">
					<text class="uni-tip-button" @click="agree">确定</text>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import {URL} from '@/common/util.js'
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	export default {
		data() {
			return {
				creatorName: '',
				createTime: '',
				name: '',
				phone: '',
				price: '',
				code: '',
				content: '',
				address: '',
				orderId: ''
			}
		},
		onLoad(e) {
			console.log(e)
			uni.request({
				url: URL + '/task/findTaskById',
				method: 'GET',
				data: {
					id: e.id
				},
				success: res => {
					console.log(res)
					this.creatorName = res.data.creatorName
					this.createTime = res.data.createTime
					this.name = res.data.name
					this.phone = res.data.creatorPhone
					this.price = res.data.reward
					this.code = res.data.code
					this.content = res.data.content
					this.address = res.data.termini
					this.orderId = res.data.id
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
			acceptOrder() {
				if( uni.getStorageSync('studentId') == '' ||  uni.getStorageSync('studentId') == null || !uni.getStorageSync('studentId')) {
					this.$refs['showtip'].open()
				} else {
					uni.request({
						url: URL + '/task/acceptTask',
						method: 'POST',
						data: {
							id: this.orderId,
							acceptorId: uni.getStorageSync('userId')
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
			},
			agree() {
				this.$refs['showtip'].close()
				uni.reLaunch({
				    url: '/pages/realName/realName'
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
		line-height: 100rpx;
		text-align: center;
		word-wrap: break-word;
		word-break: break-all;
		overflow: hidden;
	}
	
	.acceptTask {
		width: 300rpx;
		height: 100rpx;
		background: #fed803;
		color: #FFFFFF;
		text-align: center;
		line-height: 100rpx;
		position: absolute;
		left: 50%;
		bottom: 30rpx;
		transform: translate(-50%);
	}
	
	/* 提示窗口 */
	.uni-tip {
		/* #ifndef APP-NVUE */
		display: flex;
		flex-direction: column;
		/* #endif */
		padding: 15px;
		width: 250px;
		background-color: #fff;
		border-radius: 10px;
	}
	
	.uni-tip-title {
		margin-bottom: 10px;
		text-align: center;
		font-weight: bold;
		font-size: 16px;
		color: #333;
	}
	
	.uni-tip-content {
		height: 120rpx;
		text-align: center;
		font-size: 16px;
		color: #666;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.userImg {
		width: 80rpx;
		height: 80rpx;
	}
	
	.userName {
		margin-left: 20rpx;
	}
	
	.uni-tip-group-button {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		margin-top: 20px;
	}
	
	.uni-tip-button {
		flex: 1;
		text-align: center;
		font-size: 14px;
		color: #3b4144;
	}
</style>
