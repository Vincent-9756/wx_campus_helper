<template>
	<view class="content">
		<view class="content_top">
			<swiper class="swiper" indicator-dots="true" indicator-active-color="rgba(241,132,73)" autoplay="true" interval="2000" duration="500" circular="true">
				<swiper-item>
					<image style="width: 750rpx;height: 400rpx;" src="/static/images/swiper3.jpg" mode="scaleToFill"></image>
				</swiper-item>
				<swiper-item>
					<image style="width: 750rpx;height: 400rpx;" src="/static/images/swiper2.jpg" mode="scaleToFill"></image>
				</swiper-item>
				<swiper-item>
					<image style="width: 750rpx;height: 400rpx;" src="/static/images/swiper1.png" mode="scaleToFill"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="content_middle" @touchstart="start" @touchend="end">
			<view class="content_middle_top">
				<view class="top_item1" @tap="toOpenTask('EXPRESS')">
					<image class="item_img" src="/static/images/kuaidi.png" mode="aspectFit"></image>
					<text class="item_name">代拿快递</text>
				</view>
				<view class="top_item2" @tap="toOpenTask('SHOPPING')">
					<image class="item_img" src="/static/images/shop.png" mode="aspectFit"></image>
					<text class="item_name">代拿商品</text>
				</view>
			</view>
			<view class="content_middle_bottom"  @tap="toOpenTask('PACKING')">
				<view class="bottom_item">
					<image class="item_img" src="/static/images/food.png" mode="aspectFit"></image>
					<text class="item_name">代拿食堂饭菜</text>
				</view>
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
		onShow() {
			// #ifdef MP-WEIXIN
			if(wx.hideHomeButton){  
				wx.hideHomeButton();  
			}  
			// #endif
		},
		methods: {
			start(e){
			    this.clientX=e.changedTouches[0].clientX;
			},
			end(e){
			    const subX=e.changedTouches[0].clientX-this.clientX;
				if(subX<-100){
					uni.switchTab({
						url: '/pages/task/task'
					})
				}
			},
			toOpenTask (e) {
				uni.navigateTo({
					url: `/pages/openOrder/openOrder?type=${e}`
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

	.content_top {
		width: 750rpx;
		height: 400rpx;
		background: #F1F1F1;
	}
	
	.content_middle {
		flex: 1;
		background: #F1F1F1;
		display: flex;
		flex-direction: column;
		color: #57625d;
	}
	
	.content_middle_top,
	.content_middle_bottom {
		width: 750rpx;
		height: 300rpx;
		display: flex;
		justify-content: space-around;
		align-items: center;
	}
	
	.content_middle_top {
		justify-content: space-around;
	}
	
	.content_middle_bottom {
		justify-content: center;
	}
	
	.top_item1,
	.top_item2,
	.bottom_item {
		width: 300rpx;
		height: 250rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		border-radius: 8px;
	}
	
	.top_item1 {
		background: #c5effb;
	}
	
	.top_item2 {
		background:#facabc;
	}
	
	.bottom_item {
		background: #d0f8c4;
	}
	
	.item_img {
		width: 100rpx;
		height: 80rpx;
	}
	
	.item_name {
		margin-top: 10rpx;
	}
</style>
