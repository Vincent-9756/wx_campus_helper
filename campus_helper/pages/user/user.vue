<template>
	<view class="content" @touchstart="start" @touchend="end">
		<view class="content_top">
			<image class="userBg" src="/static/images/userbg.png" mode="aspectFit"></image>
			<view class="imgBox">
				<image class="avatra" :src="userInfo.avatarUrl" mode="aspectFit"></image>
			</view>
			<view class="userName">
				<text>
					{{userInfo.nickName}}
				</text>
				<text class="checkUser">
					{{autoStatus}}
				</text>
			</view>
		</view>
		<view class="content_inner">
			<view class="innerItem" v-for="(item, index) in innerArray" :key="index" @tap="toDetail(index)">
				<view class="itemLeft">
					<image class="itemPic" :src="item.img" mode="aspectFit"></image>
					<view class="itemText">
						{{item.name}}
					</view>
				</view>
				<image class="itemRignt" src="/static/images/userItem5.png" mode="aspectFit"></image>
			</view>
		</view>
		<view class="loginOut" @click="loginOut">
			<view class="itemLeft">
				<image class="itemPic" src="/static/images/userItem6.png" mode="aspectFit"></image>
				<view class="itemText">
					退出登录
				</view>
			</view>
			<image class="itemRignt" src="/static/images/userItem5.png" mode="aspectFit"></image>
		</view>
		<uni-popup ref="showtip" :type="type" :mask-click="false">
			<view class="uni-tip">
				<text class="uni-tip-title">退出登录</text>
				<view class="uni-tip-content">
					是否退出阳光雇佣兵小程序
				</view>
				<view class="uni-tip-group-button">
					<text class="uni-tip-button" @click="cancel">取消</text>
					<text class="uni-tip-button" @click="agree">确定</text>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	export default {
		data() {
			return {
				type: 'center',
				clientX: '',
				userInfo: '',
				autoStatus: '',
				innerArray: [
					{
						name: '实名认证',
						img: '/static/images/userItem1.png',
						url: '/pages/realName/realName'
					}, {
						name: '我的资料',
						img: '/static/images/userItem2.png',
						url: '/pages/myMsg/myMsg'
					}, {
						name: '已接受的任务',
						img: '/static/images/userItem3.png',
						url: ''
					}, {
						name: '已提交的任务',
						img: '/static/images/userItem4.png',
						url: ''
					}
				]
				
			}
		},
		onLoad() {
			this.userInfo = uni.getStorageSync('userInfo');
			if( uni.getStorageSync('studentId') == '' ||  uni.getStorageSync('studentId') == null || !uni.getStorageSync('studentId')) {
				this.autoStatus = '未认证'
			} else {
				this.autoStatus = '已认证'
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
						url: '/pages/order/order'
					})
				}
			},
			toDetail(e) {
				uni.navigateTo({
					url: this.innerArray[e].url
				})
			},
			loginOut() {
				this.$refs['showtip'].open()
			},
			agree() {
				uni.clearStorageSync();
				uni.redirectTo({
				    url: '/pages/login/login'
				});
			},
			cancel() {
				this.$refs['showtip'].close()
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
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		/* #endif */
		position: relative;
		background: #F1F1F1;
	}
	
	.content_top {
		width: 750rpx;
		height: 400rpx;
		position: relative;
	}
	
	.userBg {
		width: 750rpx;
		height: 400rpx;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
	}
	
	.imgBox {
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
		border: 10rpx solid #dadae4;
		position: absolute;
		top: 55%;
		left: 50%;
		transform: translate(-50%,-50%);
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.avatra {
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
	}
	
	.userName {
		display: flex;
		justify-content: center;
		position: absolute;
		left: 50%;
		bottom: 20rpx;
		color: #1f1f1f;
		transform: translate(-50%);
		font-size: 18px;
		align-items: center;
	}
	
	.checkUser {
		margin-left: 10px;
		font-size: 14px;
		color: #FF0000;
	}
	
	.content_inner {
		flex: 1;
		display: flex;
		flex-direction: column;
		color: #57625d;
		display: flex;
		flex-direction: column;
	}
	
	.innerItem {
		width: 750rpx;
		height: 120rpx;
		background: #FFFFFF;
		margin-top: 3px;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	
	.innerItem:nth-child(1) {
		margin-top: 25px;
	}
	
	.itemLeft {
		display: flex;
		margin-left: 20px;
	}
	
	.itemPic {
		width: 60rpx;
		height: 50rpx;
	}
	
	.itemText {
		font-size: 16px;
		margin-left: 20rpx;
		color: #404040;
	}
	
	.itemRignt {
		width: 50rpx;
		height: 50rpx;
		margin-right: 20px;
	}
	
	.loginOut {
		width: 750rpx;
		height: 120rpx;
		background: #FFFFFF;
		margin-top: 25px;
		display: flex;
		justify-content: space-between;
		align-items: center;
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
