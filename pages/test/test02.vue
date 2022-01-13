<!-- 手机号登录
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2022 1 13
-->
<template>
	<view>
		<!-- 导航栏 -->
		<cu-custom bgColor="bg-gradual-blue">
			<!-- <block slot="backText">返回</block> -->
			<block slot="content">登录</block>
		</cu-custom>
		
		<!-- <view v-if="autoStatus" class="login-view">
			<image class="logo-img" src="/static/touxiang.jpg"></image>
			<view class="login-btn" @click="submitlogin()">
				一键登陆
			</view>
		</view>
		<view v-else>
			不支持一键登陆功能
		</view> -->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				topList: [],
				userInfo: {},
				logState: '立即登录',
				autoStatus: true
			}
		},
		onLoad() {
			// this.isAutoLogin()
			this.submitlogin()
		},
		methods: {
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
						        "borderColor": "",  //边框颜色 默认值：透明（仅iOS支持）  
						        "borderRadius": "0px" // 其他登录按钮圆角 默认值："24px" （按钮高度的一半）
						    },  
						    "privacyTerms": {  
						        "defaultCheckBoxState":true, // 条款勾选框初始状态 默认值： true
						        "uncheckedImage":"", // 可选 条款勾选框未选中状态图片（仅支持本地图片 建议尺寸 24x24px）(3.2.0+ 版本支持)   
						        "checkedImage":"", // 可选 条款勾选框选中状态图片（仅支持本地图片 建议尺寸24x24px）(3.2.0+ 版本支持)   
						        "checkBoxSize":12, // 可选 条款勾选框大小，仅android支持
						        "textColor": "#BBBBBB", // 文字颜色 默认值：#BBBBBB  
						        "termsColor": "#5496E3", //  协议文字颜色 默认值： #5496E3  
						        "prefix": "我已阅读并同意", // 条款前的文案 默认值：“我已阅读并同意”  
						        "suffix": "并使用本机号码登录", // 条款后的文案 默认值：“并使用本机号码登录”  
						        "privacyItems": [  // 自定义协议条款，最大支持2个，需要同时设置url和title. 否则不生效  
						            {  
						                "url": "https://", // 点击跳转的协议详情页面  
						                "title": "用户服务协议" // 协议名称  
						            }  
						        ]  
						    },
						    "buttons": {  // 自定义页面下方按钮仅全屏模式生效（3.1.14+ 版本支持）
						        "iconWidth": "45px", // 图标宽度（高度等比例缩放） 默认值：45px
						        "list": [
						            {
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
									url: '../login/login_v2'
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
						if(err.index===0) {
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
			}
		}
	}
</script>

<style lang="scss">
	.login-view {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		.logo-img {
			width: 200rpx;
			height: 200rpx;
			border-radius: 50%;
			margin: 100rpx auto;
		}

		.login-btn {
			width: 50%;
			text-align: center;
			background-color: #55aaff;
			color: #FFFFFF;
			border-radius: 50rpx;
			height: 70rpx;
			line-height: 70rpx;
			font-size: 26rpx;
		}
	}
	/*登录*/
	.index-login {
		margin: auto;
		width: 379px;
	}
	
	.index-login-left {
		width: 35px;
		height: 35px;
		position: absolute;
		overflow: hidden;
		/* background: url(../../static/user.png); */
	}
	
	.index-login-left-img {
		width: 35px;
		height: 35px;
		border-radius: 50%;
	}
	
	.index-login-img-text {
		font-size: 34px;
		color: #dddddd;
	}
	
	.idnex-login-text {
		font-size: 14px;
		vertical-align: sub;
		margin-left: 45px;
		color: #888;
	}
	
	.index-login button {
		float: right;
		border: 1px #dddddd solid;
		border-radius: 30px;
	}
</style>
