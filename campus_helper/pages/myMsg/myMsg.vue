<template>
	<view class="content">
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
	export default {
		data() {
			return {
				school: '阳光学院',
				major: '软件工程',
				name: '刘鑫',
				studentId: '15100149',
				phone: '13789765783'
			}
		},
		methods: {
			
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
		background: #F1F1F1;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		height: 100%;
		/* #endif */
		position: relative;
	}
	
	.msgBox {
		width: 750rpx;
		display: flex;
		flex-direction: column;
	}
	
	.msgItem {
		width: 750rpx;
		height: 150rpx;
		display: flex;
		justify-content: space-around;
		align-items: center;
		margin-top: 20px;
		color: #57625D;
		padding-left: 20rpx;
	}
	
	.itemLeft {
		width: 200rpx;
		height: 150rpx;
		line-height: 150rpx;
		text-align: left;
	}
	
	.itemRight {
		width: 500rpx;
		height: 150rpx;
		line-height: 150rpx;
		text-align: left;
	}
</style>
