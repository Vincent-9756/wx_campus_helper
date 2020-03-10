<template>
	<view>
		<view class='headView'>
	    	<view class='headImageView'>
	      		<image class='headImage' src='/static/logo.png' mode='scaleToFill'></image>
				<view class="headName">
					阳光雇佣兵
				</view>
	    	</view>
	    	<view class='titleText'>申请获取以下权限</view>
	    	<view class='contentText'>获得你的公开信息(昵称,头像、地区等)</view>
	    	<button class='authBtn' type='primary' open-type='getUserInfo' @getuserinfo='bindGetUserInfo'>授权登录</button>
	  	</view>
		<uni-popup ref="showtip" :type="type" :mask-click="false">
			<view class="uni-tip">
				<text class="uni-tip-title">授权登录</text>
				<view class="uni-tip-content">
					<image class="userImg" :src="userImg" mode="widthFix"></image>
					<text class="userName">{{userName}}</text>
				</view>
				<view class="uni-tip-content">
					是否登录阳光雇佣兵小程序
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
	import {URL} from '@/common/util.js'
	export default {
		components:{
			uniPopup
		},
		data() {
			return {
				type: 'center',
				userImg: '',
				userName: '',
				code: '',
				userMsg: ''
			}
		},
		methods: {
			bindGetUserInfo() {
				uni.login({
					success: res_login => {
						console.log('-------获取code-------')
				      	console.log(res_login);
						this.code = res_login.code
						uni.getUserInfo({
							success: info => {
								console.log('-------获取sessionKey、openid(unionid)-------')
					            console.log(info);
								this.userImg = info.userInfo.avatarUrl
								this.userName = info.userInfo.nickName
								this.$refs['showtip'].open()
								this.userMsg = info.userInfo
				            }
				        });
				    }
				});
			},
			agree() {
				uni.request({
					url: URL + '/login',
					method: 'GET',
					data: {
						code: this.code
					},
					success: res => {
						console.log(res)
						if (res.data.msg == '授权成功') {
							uni.setStorageSync('userInfo', this.userMsg);
							uni.setStorageSync('studentId', res.data.data.studentId);
							uni.setStorageSync('userId', res.data.data.id);
							uni.switchTab({
								url: '/pages/index/index',
							})
						} else {
							uni.showToast({
								icon: 'none',
								title: '授权失败，请稍后再试！',
								duration: 3000
							})
							return false
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			cancel() {
				this.$refs['showtip'].close()
			}
		}
		
	}
</script>

<style scoped>
	.headView {
		margin: 90rpx 50rpx 90rpx 50rpx;
	}

	.headImageView {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 500rpx;
		height: 50px;
		margin: 0 auto;
	}

	.headImage{
		display: flex;
		width: 50px;
		height: 50px;
	}

	.headName {
		margin-left: 30rpx;
	}

	.titleText {
		margin-left: 25px;
		margin-top: 25px;
		margin-bottom: 10px;
		font-size: 14px;
		color: #020e0f;
		text-align: center;

	}

	.contentText {
		margin-left: 25px;
		margin-top: 0px;
		margin-bottom: 0px;
		font-size: 14px;
		color: #666;
		text-align: center;
	}

	.authBtn {
		margin-top: 35px;
		margin-left: 25px;
		margin-right: 25px;
		height: 45px;
		font-size: 17.5px;
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
