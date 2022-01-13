<!-- 测试 -->
<template>
	<view class="">
	<cu-custom bgColor="bg-gradual-blue">
		<block slot="content">查询页面</block>
	</cu-custom>
	<view class="index">

		<view class="container">
			<scroll-view scroll-y="true">
				<!--登录-->
				<view class="index-login">
					<view class="index-login-left">
						<image class="index-login-left-img" :src="userInfo.avatarUrl"></image>
					</view>
					<text class="idnex-login-text">{{ userInfo.nickName ||  '未登录' }}</text>
					<!-- #ifdef MP-WEIXIN -->
					<button type="sumbit" plain="true" size="mini" open-type="getUserInfo" @tap="login"
						@click="change">{{ logState }}</button>
					<!--#endif-->
				</view>
			</scroll-view>
		</view>
	</view>
	</view>
</template>
<script>
	// 引入css
	// import '../../common/iconfont.css'
	// 引入接口
	// import { topList } from '../../common/api.js'
	export default {
		data() {
			return {
				topList: [],
				userInfo: {},
				logState: '立即登录',
			};
		},
		onLoad() {},
		methods: {
			// 登录部分
			login() {
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
		}
		}
</script>

<style>
	/* 登录 */
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
