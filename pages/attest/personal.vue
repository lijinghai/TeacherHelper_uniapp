<!-- 个人认证页面
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 8 12
 -->
<template>
	<view>
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="true">
				<block slot="backText">返回</block>
				<block slot="content">个人认证</block>
			</cu-custom>
		</view>

		<!-- 内容 -->
		<!-- 步骤条 -->
		<view class="bg-white padding">
			<view class="cu-steps">
				<view class="cu-item" :class="index>num?'':'text-blue'" v-for="(item,index) in numList" :key="index">
					<text class="num" :class="index==2?'':''" :data-index="index + 1"></text> {{item.name}}
				</view>
			</view>
		</view>



		<view class="container">
			<u-notice-bar mode="horizontal" type="primary" :list="list"></u-notice-bar>

			<view class="cu-bar bg-white margin-top-xs">
				<view class="action sub-title">
					<text class="text-xl text-bold text-blue text-shadow">请拍摄并上传您的身份证照片</text>
					<text class="text-ABC text-blue">Driving license</text>
				</view>
			</view>

			<view class="uploadBox">
				<view style="width: 47%;">
					<view class="uploadItem">
						<view class="imgBox imgEx1">
							<view class="leftTop"></view>
							<view class="leftTop2"></view>
							<view class="leftbottom"></view>
							<view class="leftbottom2"></view>
							<view class="rightTop"></view>
							<view class="rightTop2"></view>
							<view class="rightbottom"></view>
							<view class="rightbottom2"></view>
						</view>
						<image class="imgUrl" :src="base64str"></image>
					</view>
					<view @click="uploadImg1" class="leftBtn text-white text-lg text-center">拍摄正面</view>
				</view>

				<view style="width: 47%;">
					<view class="uploadItem">
						<view class="imgBox imgEx2">
							<view class="leftTop"></view>
							<view class="leftTop2"></view>
							<view class="leftbottom"></view>
							<view class="leftbottom2"></view>
							<view class="rightTop"></view>
							<view class="rightTop2"></view>
							<view class="rightbottom"></view>
							<view class="rightbottom2"></view>
						</view>
						<image class="imgUrl" :src="imgUrl2"></image>
					</view>
					<view @click="uploadImg2" class="leftBtn text-white text-lg text-center">拍摄反面</view>
				</view>
			</view>

			<view class="cu-bar bg-white margin-top">
				<view class="action sub-title">
					<text class="text-xl text-bold text-blue text-shadow">识别内容核对</text>
					<text class="text-ABC text-blue">distinguish</text>
				</view>
			</view>

			<view class="distinguishBox">
				<view>
					<text class="text-bold disLabel">姓名：</text>
					<text>{{info.name}}</text>
				</view>
				<view>
					<text class="text-bold disLabel">民族：</text>
					<text>{{info.pople}}</text>
				</view>
				<view>
					<text class="text-bold disLabel">居住地：</text>
					<text>{{info.address}}</text>
				</view>
				<view>
					<text class="text-bold disLabel">身份证号：</text>
					<text>{{info.idCode}}</text>
				</view>
			</view>

			<view class="cu-bar bg-white margin-top">
				<view class="action sub-title">
					<text class="text-xl text-bold text-blue text-shadow">拍摄要求须知</text>
					<text class="text-ABC text-blue">requirement</text>
				</view>
			</view>

			<view class="requirement">
				<view>
					<text class="text-gray">请上传大陆公民持有的本人有效身份证；</text>
				</view>
				<view>
					<text class="text-gray">拍摄时确保身份证</text>
					<text class="text-red">边框完整，</text>
					<text class="text-red">字体清晰，</text>
					<text class="text-red">亮度均匀；</text>
				</view>

				<view class="errorBox">
					<view class="item1">
						<image class="iconImg" src="../../static/discern/ok.png" mode="widthFix"></image>
					</view>
					<view class="item2">
						<image class="iconImg" src="../../static/discern/no.png" mode="widthFix"></image>
					</view>
					<view class="item3">
						<image class="iconImg" src="../../static/discern/no.png" mode="widthFix"></image>
					</view>
					<view class="item4">
						<image class="iconImg" src="../../static/discern/no.png" mode="widthFix"></image>
					</view>
				</view>
			</view>

			<view class="submitBtn" @click="goNext">提交认证</view>

		</view>





	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 识别后的地址
				info: {
					//姓名
					name: '',
					// 民族
					pople: '',
					// 居住地
					address: '',
					// 身份证号码
					idCode: ''
				},

				// 提示文
				list: [
					'隐私说明：请按要求上传真实的证件照片，您所上传的行驶证仅用于平台账户绑定，请放心上传'
				],
				// 图片地址
				imgUrl1: '',
				imgUrl2: '',
				// api Key
				apiKey: '',
				// Secret Key
				SecretKey: '',
				// 转码后地址
				base64str: '',

				// 步骤条
				numList: [{
					name: '企业认证'
				}, {
					name: '绑定正式章'
				}, {
					name: '个人认证'
				}, {
					name: '完成'
				}, ],
				num: -1
			};
		},
		watch: {

		},
		onReady() {
			// 获取屏幕高度
			uni.getSystemInfo({
				success: res => {
					console.log(res.windowHeight)
				}
			});
		},
		mounted() {

		},
		onLoad() {
			this.getAccessToken()
			this.NumSteps(3)

			// 在百度智能云那边创建一个应用后可以获取到下面两个参数 api Key  和  Secret Key
			this.apiKey = uni.getStorageSync('gsy8FamVzqjocaerAmhbDNIj')
			this.SecretKey = uni.getStorageSync('BfrBQ9kZYL6n5DGkGGct6GyB4xOPpiUA')
		},

		// 返回页面跳转
		onUnload() {
			uni.navigateTo({
				url: './formal'
			})
		},
		methods: {

			// 调用百度的OCR获取accesToken  有效期为 2592000 秒 / 30天
			getAccessToken() {
				uni.request({
					url: 'https://aip.baidubce.com/oauth/2.0/token',
					data: {
						grant_type: 'client_credentials',
						client_id: 'gsy8FamVzqjocaerAmhbDNIj',
						client_secret: 'BfrBQ9kZYL6n5DGkGGct6GyB4xOPpiUA'
					},
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					},
					method: 'POST',
					success(res) {
						console.log("AccessToken===>")
						console.log(res)
						console.log(res.data.access_token)
						uni.setStorageSync('access_token', res.data.access_token);
					},
					fail(e) {
						uni.hideLoading()
						uni.showToast({
							title: '连接服务出错，请稍后再试!'
						})
					}
				})
			},




			//正面头像上传
			uploadImg1() {

				let that = this
				let access_token = uni.getStorageSync('access_token')

				uni.chooseImage({
					//默认9
					count: 1,
					//可以指定是原图还是压缩图，默认二者都有
					sizeType: ['original', 'compressed'],
					//从相册选择
					sourceType: ['album'],

					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						console.log(chooseImageRes);
						uni.showToast({
							icon: 'none',
							title: '上传成功',
							duration: 2000
						});
						// return false;
						uni.showLoading({
							title: '证件识别中...'
						});
						// 图片转 base64
						uni.getFileSystemManager().readFile({
							//选择图片返回的相对路径 
							filePath: tempFilePaths[0],
							//编码格式 
							encoding: 'base64',
							//成功的回调  
							success: v => {
								// 返回的是没有 'data:image/jpeg;base64,'头的数据, 需要在页面显示图片可自行追加上  
								let base64 = v.data
								that.base64str = 'data:image/jpeg;base64,' + base64

								// 开始识别
								uni.request({
									url: 'https://aip.baidubce.com/rest/2.0/ocr/v1/idcard?access_token=' +
										access_token,
									method: 'POST',
									data: {
										image: base64,
										id_card_side: 'back' // 身份证 正反面  front：身份证含照片的一面  back：身份证带国徽的一面
									},
									header: {
										'Content-Type': 'application/x-www-form-urlencoded'
									},
									success: res => {
										uni.hideLoading();
										this.info = res.data.words_result
										console.log("info===>")
										console.log(this.info)
										console.log("扫描后获取的信息")
										console.log(res.data)

										console.log(res.data.words_result.姓名.words)
										this.info.name = res.data.words_result.姓名.words

										console.log(res.data.words_result.民族.words)
										this.info.pople = res.data.words_result.民族
											.words

										console.log(res.data.words_result.住址.words)
										this.info.address = res.data.words_result.住址
											.words

										console.log(res.data.words_result.公民身份号码.words)
										this.info.idCode = res.data.words_result.公民身份号码
											.words
										console.log(this.info.name)
									}
								});
							}
						});


						uni.uploadFile({

							// url:服务器地址
							url: this.$baseUrl.baseUrl + '/upload',
							method: 'POST',

							// 获取要上传文件资源的路径
							filePath: tempFilePaths[0],
							name: 'file',
							header: {
								"Content-Type": "multipart/form-data",
								'X-Access-Token': uni.getStorageSync('bd_access_token'),
							},
							success: (uploadFileRes) => {
								console.log("进来了")
								//获取图片信息 网站域名 + res1.data.url就是一个图片的完整路径了
								var res1 = JSON.parse(uploadFileRes.data);
								console.log(res1)

								// this.form.userBaseInfo.headPhoto = JSON.parse(uploadFileRes.data)
								// 	.message
							}
						});
					}
				});
			},
			
			
			
			//反面头像上传
			uploadImg2() {
			
				let that = this
			
				uni.chooseImage({
					//默认9
					count: 1,
					//可以指定是原图还是压缩图，默认二者都有
					sizeType: ['original', 'compressed'],
					//从相册选择
					sourceType: ['album'],
			
					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						console.log(chooseImageRes);
						uni.showToast({
							icon: 'none',
							title: '上传成功',
							duration: 2000
						});
						
						uni.uploadFile({
			
							// url:服务器地址
							url: this.$baseUrl.baseUrl + '/upload',
							method: 'POST',
			
							// 获取要上传文件资源的路径
							filePath: tempFilePaths[0],
							name: 'file',
							header: {
								"Content-Type": "multipart/form-data",
								'X-Access-Token': uni.getStorageSync('bd_access_token'),
							},
							success: (uploadFileRes) => {
								console.log("进来了")
								//获取图片信息 网站域名 + res1.data.url就是一个图片的完整路径了
								var res2 = JSON.parse(uploadFileRes.data);
								this.imgUrl2 = res2.url
								console.log(res2)
		
							}
						});
					}
				});
			},
			
			

			// 提交跳转到下一页面
			goNext() {
				console.log("this.info")
				console.log(this.info.name.length)
				if(this.info.name.length != 0) {
					console.log("提交的信息======>")
					console.log(this.info.name)
					console.log(this.info.pople)
					console.log(this.info.address)
					console.log(this.info.idCode)
					
					uni.navigateTo({
						url: './over'
					})

				}else{
					console.log("进来了")
					this.$tip.error("请上传您的证件照！！")
				}
			},

			// 下一个界面激活步骤条
			myCliack() {
				//当前页
				var pages = getCurrentPages();
				//上个页面
				var beforePage = pages[pages.length - 2];
				beforePage.$vm.NumSteps(1)

				console.log("点击了")

			},

			NumSteps(step) {
				console.log("step======>" + step)
				if (step != null) {
					this.num = step - 1
					console.log('如果正处于当前第' + step + '步骤,则使用这个图片===>' + step)
				} else if (step > this.num) {
					console.log('如果该步骤比当前步骤大 也就是还没到,则展示这个图片')
				} else {
					console.log('最后就是已经过了该步骤应该展示的图片')
				}
				// this.num= this.num == this.numList.length - 1 ? 0 : this.num + 1
			}

		}
	}
</script>

<style lang="scss" scoped>
	.container {
		height: 100%;
	}

	.uploadBox {
		padding: 15rpx 20rpx 40rpx;
		background-color: #FFFFFF;
		display: flex;
		justify-content: space-between;

		.leftBtn {
			width: 100%;
			height: 75rpx;
			line-height: 75rpx;
			background-color: #007aec;
			border-radius: 0 0 12rpx 12rpx;
		}

		.uploadItem {
			width: 100%;
			height: 255rpx;
			background-color: #f1f7ff;
			border-radius: 15rpx;
			padding: 30rpx;
			position: relative;

			.imgUrl {
				width: 100%;
				height: 100%;
				position: absolute;
				left: 0;
				top: 0;
			}

			.imgBox {
				width: 100%;
				height: 100%;
				position: relative;
			}

			.imgEx1 {
				background: url(../../static/discern/ex1.png);
				background-size: 82%;
				background-repeat: no-repeat;
				background-position: center;
			}

			.imgEx2 {
				background: url(../../static/discern/ex2.png);
				background-size: 82%;
				background-repeat: no-repeat;
				background-position: center;
			}

			.leftTop {
				height: 28rpx;
				width: 4rpx;
				background-color: #007aec;
				position: absolute;
				left: 0;
			}

			.leftTop2 {
				height: 4rpx;
				width: 28rpx;
				background-color: #007aec;
				position: absolute;
				top: 0;
			}

			.leftbottom {
				height: 28rpx;
				width: 4rpx;
				background-color: #007aec;
				position: absolute;
				bottom: 0;
			}

			.leftbottom2 {
				height: 4rpx;
				width: 28rpx;
				background-color: #007aec;
				position: absolute;
				bottom: 0;
			}

			.rightTop {
				height: 28rpx;
				width: 4rpx;
				background-color: #007aec;
				position: absolute;
				right: 0;
			}

			.rightTop2 {
				height: 4rpx;
				width: 28rpx;
				background-color: #007aec;
				position: absolute;
				right: 0;
				top: 0;
			}

			.rightbottom {
				height: 28rpx;
				width: 4rpx;
				background-color: #007aec;
				position: absolute;
				right: 0;
				bottom: 0;
			}

			.rightbottom2 {
				height: 4rpx;
				width: 28rpx;
				background-color: #007aec;
				position: absolute;
				right: 0;
				bottom: 0;
			}
		}
	}

	.distinguishBox {
		padding: 5rpx 30rpx 20rpx;
		background: white;

		view {
			margin-bottom: 20rpx;
		}
	}

	.disLabel {
		text-align-last: justify;
		text-align: justify;
		text-justify: distribute-all-lines;
		min-width: 142rpx;
		display: inline-block;
		margin-right: 5rpx;
	}

	.requirement {
		padding: 10rpx 30rpx 26rpx;
		background: white;

		view {
			margin-bottom: 10rpx;
		}

		.errorBox {
			display: flex;
			justify-content: space-between;
			margin-top: 30rpx;

			view {
				width: 23%;
				height: 100rpx;
			}

			.item1 {
				background-image: url(../../static/discern/yes.png);
				background-repeat: no-repeat;
				background-size: 100%;
				position: relative;
			}

			.item2 {
				background-image: url(../../static/discern/error1.png);
				background-repeat: no-repeat;
				background-size: 100%;
				position: relative;
			}

			.item3 {
				background-image: url(../../static/discern/error2.png);
				background-repeat: no-repeat;
				background-size: 100%;
				position: relative;
			}

			.item4 {
				background-image: url(../../static/discern/error3.png);
				background-repeat: no-repeat;
				background-size: 100%;
				position: relative;
			}

			.iconImg {
				width: 60rpx;
				position: absolute;
				bottom: -10px;
				left: 50%;
				margin-left: -30rpx;
			}
		}
	}

	.submitBtn {
		width: 90%;
		height: 90rpx;
		background: #007aec;
		line-height: 90rpx;
		margin: 50rpx auto;
		text-align: center;
		font-size: 34rpx;
		color: #fff;
		border-radius: 12rpx;
	}
</style>
