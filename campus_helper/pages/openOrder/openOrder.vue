<template>
	<view class="content">
		<view class="inner">
			<view class="inner_item">
				<view class="item_title">
					任务名:
				</view>
				<view class="item_input">
					<input type="text" v-model="name" placeholder="请输入任务名" />
				</view>
			</view>
			<view class="inner_item">
				<view class="item_title">
					送达地点:
				</view>
				<view class="item_input">
					<input type="text" v-model="address" placeholder="请输入地点" />
				</view>
			</view>
			<view class="inner_item">
				<view class="item_title">
					任务金额:
				</view>
				<view class="item_input">
					<input type="text" v-model="price" placeholder="请输入金额" />
				</view>
			</view>
			<view class="inner_item" style="height: 250rpx; align-items: flex-start;">
				<view class="item_title">
					内容:
				</view>
				<view class="item_input">
					<textarea type="text" v-model="content" placeholder="请输入内容" style="padding-top: 14px;width: 245px;" />
				</view>
			</view>
		</view>
		<view class="subMsg">
			<button type="primary" class="subMsg_btn" @tap="toSubmit">
				提交任务
			</button>
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
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import {URL} from '@/common/util.js'
	export default {
		data() {
			return {
				type: '',
				name: '',
				content: '',
				address: '',
				price: '',
				creatorId: '',
			}
		},
		onLoad(e) {
			this.type = e.type
			this.creatorId = uni.getStorageSync('userId')
		},
		methods: {
			// 发布订单
			toSubmit() {
				if(this.name == '' || this.content == '' || this.address == '' || this.price == '') {
					uni.showToast({
						icon: 'loading',
						title: '内容不能为空',
						duration: 2000
					})
					return false
				}
				// 若未实名认证，前往实名认证
				if( uni.getStorageSync('studentId') == '' ||  uni.getStorageSync('studentId') == null || !uni.getStorageSync('studentId')) {
					this.$refs['showtip'].open()
				} else {
					uni.request({
						url: URL + '/task/addTask',
						method: 'POST',
						data: {
							name: this.name,
							type: this.type,
							content: this.content,
							termini: this.address,
							reward: this.price,
							creatorId: this.creatorId
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
						fail: () => {
							uni.showToast({
								icon: 'loading',
								title: res.data.msg,
								duration: 2000
							})
						},
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
	.content {
		width: 750rpx;
		/* flex: 1; */
		display: flex;
		flex-direction: column;
		background-color: #FFFFFF;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		/* height: 100%; */
		/* #endif */
		position: relative;
		color: #747474;
	}
	
	.inner {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #F1F1F1;
	}
	
	.inner_item {
		width: 750rpx;
		height: 100rpx;
		display: flex;
		align-items: center;
		background: #FFFFFF;
		margin-top: 10px;
		position: relative;
		padding-left: 20rpx;
	}
	
	.item_title {
		width: 200rpx;
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
		margin-left: 20rpx;
	}
	
	.item_input {
		height: 100rpx;
		margin-left: 20rpx;
	}
	
	.item_input>input {
		height: 100rpx;
	}
	
	.inner_item:nth-child(1) {
		margin-top: 0;
	}
	
	.subMsg {
		width: 750rpx;
		height: 150rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 20px;
	}
	
	.subMsg_btn {
		color: #ffffff!important;
		background: #3177f4!important;
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
