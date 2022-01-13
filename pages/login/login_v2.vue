<!-- 登录页面 v2 版本
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 7 30
-->
<template>
	<view>

		<!-- 导航栏 -->
		<cu-custom bgColor="bg-gradual-blue">
			<!-- <block slot="backText">返回</block> -->
			<block slot="content">登录</block>
		</cu-custom>


		<!-- 收藏小提示 -->
		<!-- #ifdef MP-WEIXIN -->
		<add-tip :tip="tip" :duration="duration" />
		<!-- #endif -->

		<!--主体-->
		<view class="zai-box">
			<scroll-view scroll-y class="page">
				<!-- 头像和文字部分 -->
				<view class="text-center" :style="[{animation: 'show ' + 0.4+ 's 1'}]">
					<image src="https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg" mode='aspectFit'
						class="zai-logo "></image>
					<view class="zai-title text-shadow ">Helper</view>

					<!-- 欢迎登录 -->
					<view class="cu-bar ">
						<view class="action sub-title">
							<text class="text-xl text-bold text-blue">欢迎登录</text>
							<text class="text-ABC text-blue">SignIn</text>
						</view>
					</view>
				</view>

				<!-- 输入框位置 -->
				<!-- 账号密码输入 -->
				<view class="box padding-lr-xl login-paddingtop" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
					<block v-if="loginWay==1">

						<!-- 账号输入框 -->
						<view class="cu-form-group margin-top  shadow-warp" :class="[shape=='round'?'round':'']">
							<view class="title"><text class="cuIcon-people margin-right-xs"></text>账号:</view>
							<input placeholder="请输入账号" name="input" v-model="userName"></input>
						</view>

						<!-- 密码输入框 -->
						<view class="cu-form-group margin-top shadow-warp" :class="[shape=='round'?'round':'']">
							<view class="title"><text class="cuIcon-lock margin-right-xs"></text>密码:</view>
							<input class="uni-input" placeholder="请输入密码" :password="!showPassword" v-model="password" />
							<view class="action text-lg">
								<text :class="[showPassword ? 'cuIcon-attention' : 'cuIcon-attentionforbid']"
									@click="changePassword"></text>
							</view>
						</view>

						<!-- 登录按钮 -->
						<view class="padding text-center margin-top">
							<button class="cu-btn bg-blue lg margin-right shadow" :loading="loading"
								:class="[shape=='round'?'round':'']" @tap="onLogin"><text
									space="emsp">{{loading ? "登录中...":" 登录 "}}</text>
							</button>

							<button class="cu-btn line-blue lg margin-left shadow" :loading="loading"
								@tap="submitlogin()">短信注册
							</button>

							<!-- <button class="cu-btn line-blue lg margin-left shadow" :loading="loading"
								:class="[shape=='round'?'round':'']" @tap="loginWay=3-loginWay">短信注册
							</button> -->
						</view>

						<!-- 找回密码 -->
						<!-- <view style="right: 0rpx;">找回密码</view> -->
					</block>


					<!-- 短信登录 -->
					<block v-else>


						<!-- 手机号输入框 -->
						<view class="cu-form-group margin-top  shadow-warp" :class="[shape=='round'?'round':'']">
							<view class="title"><text class="cuIcon-mobile margin-right-xs"></text>手机号:</view>
							<input placeholder="请输入手机号" type="number" maxlength="11" v-model="phoneNo"></input>
						</view>

						<!-- 验证码输入框 -->
						<view class="cu-form-group margin-top shadow-warp" :class="[shape=='round'?'round':'']">
							<view class="title"><text class="cuIcon-lock margin-right-xs"></text>验证码:</view>
							<input class="uni-input" placeholder="请输入验证码" v-model="smsCode" />
							<view class="action">
								<button class="cu-btn line-blue sm" :disabled="!isSendSMSEnable" @click="onSMSSend">
									{{ getSendBtnText }}</button>
							</view>
						</view>
						<!-- <view>未注册的手机号验证后自动创建账号</view> -->

						<!-- 登录按钮 -->
						<view class="padding text-center margin-top">
							<button class="cu-btn bg-blue lg margin-right shadow" :loading="loading"
								:class="[shape=='round'?'round':'']" @tap="onSMSLogin"><text
									space="emsp">{{loading ? "登录中...":" 登录 "}}</text>
							</button>
							<button class="cu-btn line-blue lg margin-left shadow" :loading="loading"
								:class="[shape=='round'?'round':'']" @tap="loginWay=1">账号登录
							</button>
						</view>


					</block>

					<!-- #ifdef APP-PLUS
					<view class="padding flex flex-direction  text-center">
						当前版本:{{version}}
					</view>
					<!-- #endif -->

				</view>
			</scroll-view>

			<!-- 登录加载弹窗 -->
			<view class="cu-load load-modal" v-if="loading">
				<!-- <view class="cuIcon-emojifill text-orange"></view> -->
				<image src="https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg" mode="aspectFit"
					class="round"></image>
				<view class="gray-text">登录中...</view>
			</view>

			<!-- QQ、微信登录按钮 -->
			<view class="buttom">
				<view class="loginType">
					<view class="wechat item">
						<view class="icon" @tap="wxlogin()" @click="change" open-type="getUserInfo" type="sumbit">
							<u-icon size="70" name="weixin-fill" color="rgb(83,194,64)"></u-icon>
						</view>
						微信
					</view>
					<view class="QQ item">
						<view class="icon">
							<u-icon size="70" name="qq-fill" color="rgb(17,183,233)"></u-icon>
						</view>
						QQ
					</view>
				</view>
				<view class="hint">
					登录代表同意
					<text class="link">用户协议、隐私政策，</text>
					并授权使用您的账号信息（如昵称、头像、收获地址）以便您统一管理
				</view>
			</view>

		</view>

	</view>

</template>

<script>
	import request from '@/utils/myRequest.js';
	// import { ACCESS_TOKEN,USER_NAME,USER_INFO } from "@/common/util/constants"
	import {
		mapActions
	} from "vuex"
	// import configService from '@/common/service/config.service.js';

	// 导入添加收藏提示组件
	import addTip from '../../components/wxcomponents/struggler-uniapp-add-tip/struggler-uniapp-add-tip.vue';

	export default {
		// 导入收藏提示组件
		components: {
			addTip
		},
		data() {
			return {
				tip: '点击「添加小程序」，下次访问更便捷',
				duration: 1,

				shape: '', //round 圆形
				loading: false,
				userName: '1',
				password: '1',
				phoneNo: '',
				smsCode: '',
				showPassword: false, //是否显示明文
				loginWay: 1, //1: 账密，2：验证码
				smsCountDown: 0,
				smsCountInterval: null,
				toggleDelay: false,
				version: '',
				//第三方登录相关信息
				thirdType: "",
				thirdLoginInfo: "",
				thirdLoginState: false,
				bindingPhoneModal: false,
				thirdUserUuid: '',
				// url: {
				// 	bindingThirdPhone: '/sys/thirdLogin/bindingThirdPhone'
				// }
			};
		},
		onLoad: function() {
			// #ifdef APP-PLUS
			var that = this
			plus.runtime.getProperty(plus.runtime.appid, function(wgtinfo) {
				that.version = wgtinfo.version
			});
			// #endif
		},
		computed: {
			isSendSMSEnable() {
				return this.smsCountDown <= 0 && this.phoneNo.length > 4;
			},
			getSendBtnText() {
				if (this.smsCountDown > 0) {
					return this.smsCountDown + '秒后发送';
				} else {
					return '发送验证码';
				}
			},
			canSMSLogin() {
				return this.userName.length > 4 && this.smsCode.length > 4;
			},
			canPwdLogin() {
				return this.userName.length > 4 && this.password.length > 4;
			},
		},
		methods: {
			...mapActions(["mLogin", "PhoneLogin", "ThirdLogin"]),

			// 账号密码登录请求
			onLogin: function() {
				if (!this.userName || this.userName.length == 0) {
					this.$tip.toast('请填写用户名');
					return;
				}
				if (!this.password || this.password.length == 0) {
					this.$tip.toast('请填写密码');
					return;
				}
				let loginParams = {
					username: this.userName,
					password: this.password
				}
				this.loading = true;

				const _this = this // 获取此时的this为一个常量，防止下面请求回调改变出错
				console.log("表单提交")
				let opts = {
					url: '/pcuser/login',
					method: 'POST',
				};
				let data = loginParams;

				this.$httpRequest(opts, data).then((res) => {
					console.log(res)
					this.loading = false;
					if (res.data.code === 20000) { // 获取数据成功
						console.log("成功")
						uni.setStorageSync('token', res.data.data.token); // 将登录信息以token的方式存在手机硬盘中

						console.log("token" + res.data.data.token)
						uni.switchTab({
							url: '../work/index'
						})
						this.$tip.success('登录成功!')
					} else if (res.data.code === 500) { // 获取数据失败
						console.log("失败")
						this.loading = false;
						this.$tip.alert(res.data.message);
					}
				}).catch((err) => {
					let msg = "请求出现错误，请稍后再试"
					this.loading = false;
					this.$tip.alert(msg);
				}).finally(() => {
					this.loading = false;
				});

				// let opts = {
				// 	url: '/pcuser/info?token='+ uni.getStorageSync('token'),
				// 	method: 'GET'
				// };
				// uni.showLoading({
				// 	title: '加载中'
				// });
				// this.$httpTokenRequest(opts).then(res => {
				// 	console.log("执行了=========>")
				// 	console.log(res);
				// 	uni.hideLoading();
				// 	});

			},



			saveClientId() {
				var info = plus.push.getClientInfo();
				var cid = info.clientid;
				this.$http.get("/sys/user/saveClientId", {
					params: {
						clientId: cid
					}
				}).then(res => {
					console.log("res::saveClientId>", res)
					this.$tip.success('登录成功!')
					this.$Router.replaceAll({
						name: 'index'
					})
				})
			},
			changePassword() {
				this.showPassword = !this.showPassword;
			},
			onSMSSend() {
				let smsParams = {};
				smsParams.mobile = this.phoneNo;
				smsParams.smsmode = "0";
				let checkPhone = new RegExp(/^[1]([3-9])[0-9]{9}$/);
				if (!smsParams.mobile || smsParams.mobile.length == 0) {
					this.$tip.toast('请输入手机号');
					return false
				}
				if (!checkPhone.test(smsParams.mobile)) {
					this.$tip.toast('请输入正确的手机号');
					return false
				}
				this.$http.post("/sys/sms", smsParams).then(res => {
					if (res.data.success) {
						this.smsCountDown = 60;
						this.startSMSTimer();
					} else {
						this.smsCountDown = 0;
						this.$tip.toast(res.data.message);
					}
				});
			},
			startSMSTimer() {
				this.smsCountInterval = setInterval(() => {
					this.smsCountDown--;
					if (this.smsCountDown <= 0) {
						clearInterval(this.smsCountInterval);
					}
				}, 1000);
			},
			onSMSLogin() {
				let checkPhone = new RegExp(/^[1]([3-9])[0-9]{9}$/);

				if (!this.phoneNo || this.phoneNo.length == 0) {
					this.$tip.toast('请填写手机号');
					return;
				}
				if (!checkPhone.test(this.phoneNo)) {
					this.$tip.toast('请输入正确的手机号');
					return false
				}
				if (!this.smsCode || this.smsCode.length == 0) {
					this.$tip.toast('请填短信验证码');
					return;
				}
				let loginParams = {
					mobile: this.phoneNo,
					captcha: this.smsCode
				};
				this.PhoneLogin(loginParams).then((res) => {
					console.log("res====》", res)
					if (res.data.success) {
						this.$tip.success('登录成功!')
						this.$Router.replaceAll({
							name: 'index'
						})
					} else {
						this.$tip.error(res.data.message);
					}
				}).catch((err) => {
					let msg = ((err.response || {}).data || {}).message || err.data.message || "请求出现错误，请稍后再试"
					this.$tip.error(msg);
				});
			},
			loginSuccess() {
				// 登陆成功，重定向到主页
				this.$Router.replace({
					name: 'index'
				})
			},
			requestFailed(err) {
				this.$message.warning("登录失败")
			},
			// 判断是否支持一键登陆
			isAutoLogin() {
				let _that = this
				uni.getProvider({ //获取可用的服务提供商
					service: 'oauth',
					success: function(res) {
						console.log(res.provider) // ['weixin', qq', 'univerify']
					}
				});
				uni.preLogin({ //预登录
					provider: 'univerify', //用手机号登录
					success() {
						_that.autoStatus = true
						console.log('预登录成功')
					},

					fail(err) { //预登录失败
						_that.autoStatus = false
						console.log('错误码：' + err.errCode)
						console.log(err.errMsg)
					}
				})
			},
			submitlogin() {
				uni.login({ //正式登录，弹出授权窗
					provider: 'univerify',
					univerifyStyle: { // 自定义登录框样式
						"fullScreen": true, // 是否全屏显示，true表示全屏模式，false表示非全屏模式，默认值为false。
						"backgroundColor": "#ffffff", // 授权页面背景颜色，默认值：#ffffff 
						"icon": {
							"path": "static/touxiang.jpg" // 自定义显示在授权框中的logo，仅支持本地图片 默认显示App logo   
						},
						"phoneNum": {
							"color": "#2281F5", // 手机号文字颜色 默认值：#000000   
						},
						"authButton": {
							"normalColor": "#3479f5", // 授权按钮正常状态背景颜色 默认值：#3479f5  
							"highlightColor": "#2861c5", // 授权按钮按下状态背景颜色 默认值：#2861c5（仅ios支持）  
							"disabledColor": "#73aaf5", // 授权按钮不可点击时背景颜色 默认值：#73aaf5（仅ios支持）  
							"textColor": "#ffffff", // 授权按钮文字颜色 默认值：#ffffff  
							"title": "本机号码一键登录" // 授权按钮文案 默认值：“本机号码一键登录”  
						},
						"otherLoginButton": {
							"visible": false, // 是否显示其他登录按钮，默认值：true  
							"normalColor": "", // 其他登录按钮正常状态背景颜色 默认值：透明 
							"highlightColor": "", // 其他登录按钮按下状态背景颜色 默认值：透明 
							"textColor": "#656565", // 其他登录按钮文字颜色 默认值：#656565  
							"title": "其他登录方式", // 其他登录方式按钮文字 默认值：“其他登录方式”  
							"borderColor": "", //边框颜色 默认值：透明（仅iOS支持）  
							"borderRadius": "0px" // 其他登录按钮圆角 默认值："24px" （按钮高度的一半）
						},
						"privacyTerms": {
							"defaultCheckBoxState": true, // 条款勾选框初始状态 默认值： true
							"uncheckedImage": "", // 可选 条款勾选框未选中状态图片（仅支持本地图片 建议尺寸 24x24px）(3.2.0+ 版本支持)   
							"checkedImage": "", // 可选 条款勾选框选中状态图片（仅支持本地图片 建议尺寸24x24px）(3.2.0+ 版本支持)   
							"checkBoxSize": 12, // 可选 条款勾选框大小，仅android支持
							"textColor": "#BBBBBB", // 文字颜色 默认值：#BBBBBB  
							"termsColor": "#5496E3", //  协议文字颜色 默认值： #5496E3  
							"prefix": "我已阅读并同意", // 条款前的文案 默认值：“我已阅读并同意”  
							"suffix": "并使用本机号码登录", // 条款后的文案 默认值：“并使用本机号码登录”  
							"privacyItems": [ // 自定义协议条款，最大支持2个，需要同时设置url和title. 否则不生效  
								{
									"url": "https://", // 点击跳转的协议详情页面  
									"title": "用户服务协议" // 协议名称  
								}
							]
						},
						"buttons": { // 自定义页面下方按钮仅全屏模式生效（3.1.14+ 版本支持）
							"iconWidth": "45px", // 图标宽度（高度等比例缩放） 默认值：45px
							"list": [{
									"provider": "apple",
									"iconPath": "/static/wx.png" // 图标路径仅支持本地图片
								}
								// {
								//     "provider": "weixin",
								//     "iconPath": "/static/touxiang.jpg" // 图标路径仅支持本地图片
								// }
							]
						}
					},
					success(res) { // 正式登录成功
						console.log(res.authResult); // {openid:'登录授权唯一标识',access_token:'接口返回的 token'}

						// 在得到access_token后，通过callfunction调用云函数
						uniCloud.callFunction({
							name: 'loginByUniverify', // 云函数名称
							data: { //传给云函数的参数
								'access_token': res.authResult.access_token, // 客户端一键登录接口返回的access_token
								'openid': res.authResult.openid // 客户端一键登录接口返回的openid
							},
							success(callRes) {
								console.log('调用云函数成功' + callRes)
								uni.switchTab({
									url: '../index/index'
								})
							},
							fail(callErr) {
								console.log('调用云函数出错' + callErr)
							},
							complete() {
								uni.closeAuthView() //关闭授权登录界面
							}
						})
					},
					fail(err) { // 正式登录失败
						console.log(err.errCode)
						console.log(err.errMsg)
						console.log(err.index)
						if (err.index === 0) {
							// 获取code 小程序专有，用户登录凭证。
							uni.getUserProfile({
								desc: "获取用户基本资料",
								success: (res) => {
									this.userInfo = res.userInfo;
									console.log("res")
									console.log(res)
								},
								// 用户取消登录后的提示
								// fail: (res) => {
								// 	uni.showModal({
								// 		title: "授权用户信息失败！",
								// 		// 是否显示取消按钮，默认为 true
								// 		showCancel: false
								// 	})
								// }
							})
							//获取成功基本资料后开启登录，基本资料首先要授权
							uni.login({
								provider: 'weixin',
								success: (res) => {
									console.log("res11");
									console.log(res);
									if (res.errMsg == "login:ok") {
										let code = res.code;
									}
								}
							})
						}
						uni.closeAuthView() //关闭授权登录界面
					}
				})
			},
			// 微信登录部分
			wxlogin() {
				// 获取code 小程序专有，用户登录凭证。
				uni.getUserProfile({
					desc: "获取用户基本资料",
					success: (res) => {
						this.userInfo = res.userInfo;
						console.log("res")
						console.log(res)
					},
					// 用户取消登录后的提示
					fail: (res) => {
						// uni.showModal({
						// 	title: "授权用户信息失败！",
						// 	// 是否显示取消按钮，默认为 true
						// 	showCancel: false
						// })
					}
				})
				//获取成功基本资料后开启登录，基本资料首先要授权
				uni.login({
					provider: 'weixin',
					success: (res) => {
						console.log("res11");
						console.log(res);
						console.log(userInfo);
						if (res.errMsg == "login:ok") {
							let code = res.code;
						}
					}
				})
			},
			// 退出登录
			change() {
				// 这里只是改变了按钮文字内容，真正退出需要清除token，回到首页,还没找到头绪怎么做
				this.logState = '已登录';
			}

		},
		beforeDestroy() {
			if (this.smsCountInterval) {
				clearInterval(this.smsCountInterval);
			}
		},
	}
</script>

<style lang="scss" scoped>
	/* 输入框高度位置 */
	.login-paddingtop {
		/* padding-top: 100upx; */
	}

	.zai-box {
		padding: 0 20upx;
		padding-top: 100upx;
		position: relative;

		.buttom {
			.loginType {
				display: flex;
				// padding: 350rpx 150rpx 150rpx 150rpx;
				padding: 18rpx 150rpx 150rpx 150rpx;
				justify-content: space-between;

				.item {
					display: flex;
					flex-direction: column;
					align-items: center;
					color: $u-content-color;
					font-size: 28rpx;
				}
			}

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}
	}

	.zai-logo {
		width: 200upx;
		// height: 150px;
		height: 94px;
	}

	.zai-title {
		font-size: 58upx;
		color: #000000;
		text-align: center;
	}

	.input-placeholder,
	.zai-input {
		color: #94afce;
	}

	.zai-label {
		padding: 60upx 0;
		text-align: center;
		font-size: 30upx;
		color: #a7b6d0;
	}

	.zai-btn {
		background: #ff65a3;
		color: #fff;
		border: 0;
		border-radius: 100upx;
		font-size: 36upx;
	}

	.zai-btn:after {
		border: 0;
	}

	/*按钮点击效果*/
	.zai-btn.button-hover {
		transform: translate(1upx, 1upx);
	}
</style>
