<template>
	<view class="content">
		<view class="content_top">
			<image class="top_pic" src="/static/images/realName.png" mode="aspectFit"></image>
		</view>
		<view class="content_middle">
			<view class="minddle_title">
				填写资料
			</view>
			<view class="inputBox">
				<view class="inputBox_item">
					<view class="itemLeft">
						学校：
					</view>
					<view class="itemRight" style="width: 500rpx;">
						<xfl-select
							:list="schoolList"
							:clearable="true"
							:showItemNum="5" 
							:listShow="false"
							:isCanInput="false"  
							:style_Container="'height: 50px; font-size: 16px;'"
							:placeholder = "'请选择学校'"
							:selectHideType="'hideAll'"
							@change="selectSchool"
						>
						</xfl-select>
					</view>
				</view>
				<view class="inputBox_item">
					<view class="itemLeft">
						专业：
					</view>
					<view class="itemRight" style="width: 500rpx;">
						<xfl-select
							:list="majorList"
							:clearable="true"
							:showItemNum="5" 
							:listShow="false"
							:isCanInput="false"  
							:style_Container="'height: 50px; font-size: 16px;'"
							:placeholder = "'请选择专业'"
							:selectHideType="'hideAll'"
							@change="selectMajor"
						>
						</xfl-select>
					</view>
				</view>
				<view class="inputBox_item">
					<view class="itemLeft">
						姓名：
					</view>
					<view class="itemRight">
						<input class="inputItem" type="text" v-model="name" placeholder="请输入姓名"/>
					</view>
				</view>
				<view class="inputBox_item">
					<view class="itemLeft">
						学号：
					</view>
					<view class="itemRight">
						<input class="inputItem" type="text" v-model="code" placeholder="请输入学号"/>
					</view>
				</view>
				<view class="inputBox_item">
					<view class="itemLeft">
						手机号：
					</view>
					<view class="itemRight">
						<input class="inputItem" type="text" v-model="phone" placeholder="请输入手机号"/>
					</view>
					<view class="getNum" @tap="getNum" v-if="showGetNum">
						获取验证码
					</view>
					<view class="getNum" v-if="!showGetNum">
						{{btnTitle}}
					</view>
				</view>
				<view class="inputBox_item">
					<view class="itemLeft">
						验证码：
					</view>
					<view class="itemRight">
						<input class="inputItem" type="text" v-model="num" placeholder="请输入验证码"/>
					</view>
				</view>
			</view>
		</view>
		<view class="content_bottom" v-if="!isShow">
			<button type="primary" class="submitMsg" @tap="submitMsg">
				提交信息
			</button>
		</view>
	</view>
</template>

<script>
	import xflSelect from '../../components/xfl-select/xfl-select.vue';
	import {URL} from '@/common/util.js'
	export default {
		data() {
			return {
				btnTitle: '',
				schoolId: '',
				major: '',
				code: '',
				name: '',
				studentId: '',
				phone: '',
				num: '',
				showGetNum: true,
				schoolList: [],
				majorList: [],
				isShow: false
			}
		},
		components: {
			xflSelect
		},
		onShow() {
			// #ifdef MP-WEIXIN  
			if(wx.hideHomeButton){  
			    wx.hideHomeButton();  
			}  
			// #endif
		},
		onLoad() {
			if( uni.getStorageSync('studentId') == '' ||  uni.getStorageSync('studentId') == null || !uni.getStorageSync('studentId')) {
				this.isShow = false
			} else {
				this.isShow = true
			}
			this.schoolList.splice(0)
			// 获取学校下拉框
			uni.request({
				url: URL + '/school/querySchool',
				method: 'POST',
				data: {},
				success: res => {
					console.log(res)
					res.data.data.forEach(e=> {
						this.schoolList.push({
							value: e.name,
							index: e.id,
						})
					})
					
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			// 获取手机验证码
			getNum() {
				if(!this.phone) {
					uni.showToast({
					    title: '手机号不能为空',
						icon: 'none',
					    duration: 2000
					});
					return false
				} else if(!/^1[345678]\d{9}$/.test(this.phone)) {
					uni.showToast({
					    title: '手机号格式不正确',
						icon: 'none',
					    duration: 2000
					});
					return false
				}
				//倒计时
				let time = 60;
				let timer = setInterval(() => {
					if(time == 0) {
						clearInterval(timer);
						this.showGetNum = true
					} else {
						this.btnTitle =time + '秒后重试';
						this.showGetNum = false
						time--
					}
				 },1000)
				 uni.request({
				 	url: URL + '/sendMs',
				 	method: 'GET',
				 	data: {
				 		phone: this.phone
				 	},
				 	success: res => {
				 		console.log(res)
				 		
				 	},
				 	fail: () => {},
				 	complete: () => {}
				 });
			},
			// 选取学校来获取专业
			selectSchool({newVal, oldVal, index, orignItem}) {
				this.majorList.splice(0)
				console.log(uni.getStorageSync('studentId'))
				console.log(newVal, oldVal, index, orignItem)
				console.log(this.schoolList[index].index)
				this.schoolId = this.schoolList[index].index
				uni.request({
					url: URL + '/school/queryMajor',
					method: 'GET',
					data: {
						schoolId: this.schoolId
					},
					success: res => {
						console.log(res)
						res.data.forEach(e=> {
							this.majorList.push({
								value: e.major,
							})
						})
						
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 选取专业
			selectMajor({newVal, oldVal, index, orignItem}) {
				console.log(newVal, oldVal, index, orignItem)
				console.log(this.schoolList[index].index)
				this.major = newVal
			},
			// 发送实名信息
			submitMsg() {
				console.log(uni.getStorageSync('userId'))
				uni.request({
					url: URL + '/authentication',
					method: 'POST',
					data: {
						id: uni.getStorageSync('userId'),
						major: this.major,
						code: this.code,
						name: this.name,
						phone: this.phone,
						num: this.num,
						schoolId: this.schoolId
					},
					success: res => {
						console.log(res)
						if (res.data.result == "SUCCESS") {
							uni.showToast({
								icon: 'none',
								title: res.data.msg,
								duration: 3000
							})
							uni.setStorageSync('studentId', res.data.data.studentId);
							uni.reLaunch({
								url: '/pages/user/user'
							})
						} else {
							uni.showToast({
								icon: 'none',
								title: res.data.msg,
								duration: 3000
							})
							return false
						}
						
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.content {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		background: #F1F1F1;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		/* #endif */
		position: relative;
	}

	.content_top {
		width: 750rpx;
		height: 400rpx;
		position: relative;
		background: #F1F1F1;
	}
	
	.top_pic {
		width: 750rpx;
		height: 400rpx;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
	}
	
	.content_middle {
		width: 750rpx;
		display: flex;
		flex-direction: column;
	}
	
	.minddle_title {
		width: 750rpx;
		height: 100rpx;
		background: #F1F1F1;
		line-height: 100rpx;
		text-align: center;
		color: #818181;
	}
	
	.inputBox {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #F1F1F1;
	}
	
	.inputBox_item {
		width: 750rpx;
		height: 100rpx;
		display: flex;
		align-items: center;
		background: #FFFFFF;
		margin-top: 10px;
		position: relative;
		padding-left: 20rpx;
	}
	
	.itemLeft {
		width: 150rpx;
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
	}
	
	.itemRignt {
		width: 350rpx;
		height: 100rpx;
	}
	
	.inputBox_item:nth-child(1) {
		margin-top: 0;
	}
	
	.getNum {
		height: 100rpx;
		line-height: 100rpx;
		color: #808080;
		font-size: 12px;
	}
	
	.content_bottom {
		width: 750rpx;
		height: 150rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	
	.submitMsg {
		color: #ffffff!important;
		background: #3177f4!important;
	}
</style>
