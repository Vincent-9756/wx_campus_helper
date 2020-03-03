<template>
	<view class="content">
		<view class="content_item">
			<view class="item_top">
				<view class="taskType">
					任务：帮买食堂饭菜
				</view>
				<view class="taskAddress">
					<view class="addressText">
						地址：{{address}}
					</view>
					<image src="/static/images/addr.png" mode="aspectFit"></image>
				</view>
				<view class="taskPrice">
					佣金：{{price}}
				</view>
			</view>
			<view class="item_bottom" @tap="toAccept">
				<view class="accpetTask">
					查看详情
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				price: '15',
				address: '西区10号楼715'
			}
		},
		methods: {
			toAccept() {
				uni.navigateTo({
					url: '/pages/helpOrder/helpFoodDetail'
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
</style>
