<template>
	<view class="content">
		<view class="content_item">
			<view class="item_top">
				<view class="taskType">
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
				订单编号：{{code}}
			</view>
			<view class="detailItem">
				食品名称：{{kNum}}
			</view>
			<view class="detailItem">
				地址：{{address}}
			</view>
		</view>
		<view class="acceptTask">
			接单
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone: '15761665647',
				price: '15',
				code: '2220220200202',
				kNum: '21210120',
				address: '西区10号楼715'
			}
		},
		methods: {
			toCall() {
				uni.makePhoneCall({
				    phoneNumber: this.phone
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
</style>
