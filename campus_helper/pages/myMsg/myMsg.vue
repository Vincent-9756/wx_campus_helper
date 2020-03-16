<template>
	<view class="content">
		<view class="content_top">
			<image class="topBanner" src="/static/images/userInfo.jpg" mode="scaleToFill"></image>
			<image class="avatra" :src="userInfo.avatarUrl" mode="aspectFit"></image>
			<view class="userName">
				{{userInfo.nickName}}
			</view>
		</view>
		<view class="msgBox">
			<view class="msgItem">
				<view class="itemLeft">
					学校：
				</view>
				<view class="itemRight">
					{{school}}
				</view>
			</view>
			<view class="msgItem">
				<view class="itemLeft">
					专业：
				</view>
				<view class="itemRight">
					{{major}}
				</view>
			</view>
			<view class="msgItem">
				<view class="itemLeft">
					姓名：
				</view>
				<view class="itemRight">
					{{name}}
				</view>
			</view>
			<view class="msgItem">
				<view class="itemLeft">
					学号：
				</view>
				<view class="itemRight">
					{{studentId}}
				</view>
			</view>
			<view class="msgItem">
				<view class="itemLeft">
					手机号：
				</view>
				<view class="itemRight">
					{{phone}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {URL} from '@/common/util.js'
	export default {
		data() {
			return {
				userInfo: '',
				school: '',
				major: '',
				name: '',
				studentId: '',
				phone: ''
			}
		},
		onLoad() {
			// 调取接口 获取个人信息
			this.userInfo = uni.getStorageSync('userInfo');
			uni.request({
				url: URL + '/myInfo',
				method: 'GET',
				data: {
					id: uni.getStorageSync('userId')
				},
				success: res => {
					console.log(res)
					this.school = res.data.schoolName
					this.major = res.data.major
					this.name = res.data.name
					this.studentId = res.data.code
					this.phone = res.data.phone
				},
				fail: () => {},
				complete: () => {}
			});
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
		background: #FFFFFF;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		/* height: 100%; */
		/* #endif */
		position: relative;
	}
	
	.content_top {
		width: 750rpx;
		height: 400rpx;
		position: relative;
	}
	
	.topBanner {
		width: 750rpx;
		height: 400rpx;
		position: absolute;
		top: 0;
		left: 0;
	}
	
	.avatra {
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	
	.userName {
		color: #FFFFFF;
		font-size: 18px;
		position: absolute;
		bottom: 60rpx;
		left: 50%;
		transform: translate(-50%);
	}
	
	.msgBox {
		width: 750rpx;
		display: flex;
		flex-direction: column;
	}
	
	.msgItem {
		width: 750rpx;
		display: flex;
		justify-content: space-around;
		align-items: center;
		justify-content: center;
		margin-top: 20px;
		padding-left: 20rpx;
	}
	
	.itemLeft {
		width: 200rpx;
		text-align: left;
		color: #808080;
	}
	
	.itemRight {
		width: 500rpx;
		text-align: left;
		color: #000000;
	}
</style>
