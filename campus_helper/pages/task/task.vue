<template>
	<view class="content" @touchstart="start" @touchend="end">
		<image class="bgPic" src="/static/images/back.png" mode="aspectFit"></image>
		<view class="helpBox">
			<view class="helpItem" @tap="toHelpOrder('EXPRESS')">
				<button type="primary">帮拿快递</button>
			</view>
			<view class="helpItem" @tap="toHelpOrder('SHOPPING')">
				<button type="primary">帮拿商品</button>
			</view>
			<view class="helpItem" @tap="toHelpOrder('PACKING')">
				<button type="primary">帮拿食堂饭菜</button>
			</view>
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
				// 若subX>100，回上一页面
				if(subX>100){
					uni.switchTab({
						url: '/pages/index/index'
					})
				} else if(subX<-100){
					// 若subX<100，到下一页面
					uni.switchTab({
						url: '/pages/order/order'
					})
				}
			},
			// 前往接单页面
			toHelpOrder(e) {
				uni.navigateTo({
					url: `/pages/helpOrder/helpOrder?type=${e}`
				})
			},
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
	
	.bgPic {
		width: 750rpx;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
	}
	
	.helpBox {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		position: absolute;
		bottom: 50rpx;
	}
	
	.helpItem {
		width: 750rpx;
		height: 100rpx;
		margin-top: 10px;
		display: flex;
		justify-content: center;
	}
	
	.helpItem>button {
		width: 300rpx;
		height: 100rpx;
		background: #f5c253;
	}
</style>
