<template>
	<view class="content">
		<view class="inner">
			<view class="inner_item" style="height: 200rpx;">
				<view class="item_title" style="height: 200rpx; line-height: none;">
					商品内容:
				</view>
				<view class="item_input textInput">
					<textarea v-model="shop" placeholder="请输入商品内容" />
				</view>
			</view>
			<view class="inner_item">
				<view class="item_title">
					联系人:
				</view>
				<view class="item_input">
					<input type="text" v-model="name" placeholder="请输入联系人" />
				</view>
			</view>
			<view class="inner_item">
				<view class="item_title">
					联系电话:
				</view>
				<view class="item_input">
					<input type="text" v-model="phone" placeholder="请输入手机号" />
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
		</view>
		<view class="subMsg">
			<button type="primary" class="subMsg_btn">
				提交任务
			</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shop: '',
				name: '',
				phone: '',
				address: '',
				price: ''
			}
		},
		methods: {
			
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
	
	.textInput {
		width: 500rpx;
		height: 200rpx;
		overflow: scroll;
	}
	
	.textInput>textarea {
		width: 500rpx;
		height: 200rpx;
		padding-top: 25rpx;
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
</style>
