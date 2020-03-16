<template>
	<view class="content" @touchstart="start" @touchend="end">
		<view class="content_top" @tap="toOpenList">
			<image src="/static/images/order2.png" mode="scaleToFill"></image>
		</view>
		<view class="content_bottom" @tap="toAcceptList">
			<image src="/static/images/order1.png" mode="scaleToFill"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				clientX: ''
			}
		},
		methods: {
			start(e){
				console.log(e.changedTouches[0].clientX)
			    this.clientX=e.changedTouches[0].clientX;
			},
			end(e){
			    const subX=e.changedTouches[0].clientX-this.clientX;
				console.log(subX)
				if(subX>100){
					uni.switchTab({
						url: '/pages/task/task'
					})
				} else if(subX<-100){
					uni.switchTab({
						url: '/pages/user/user'
					})
				}
			},
			// 前往我接受的订单页面
			toAcceptList() {
				uni.navigateTo({
					url: '/pages/order/acceptOrderList'
				})
			},
			// 前往我发布的订单页面
			toOpenList() {
				uni.navigateTo({
					url: '/pages/order/openOrderList'
				})
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
		background-color: #ffffff;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		height: 100%;
		/* #endif */
		position: relative;
	}
	
	.content_top,
	.content_bottom {
		width: 750rpx;
		height: 50%;
		position: relative;
	}
	
	.content_top>image,
	.content_bottom>image {
		width: 750rpx;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
	}
</style>
