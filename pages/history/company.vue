<!-- 拍照页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 4
-->
<template>
	<view>
		<view>

			<!-- 照片上的信息 -->
			<camera :device-position="device" :flash="flash" @error="error"
				:style="{ width: '100%',position: 'relative', height: getHeight + 'px' }">
				<cover-view class="topBox">
					<!-- <cover-view class="topItem text-bold text-xl">经办人姓名：{{info.username}}</cover-view> -->
					<cover-view class="topItem">{{nowTime2}}</cover-view>
					<cover-view class="topItem">{{nowTime}}</cover-view>
					<cover-view class="topItem">{{address}}</cover-view>
				</cover-view>

				<!-- 翻转摄像头 -->
				<cover-image @click="xzBtn" class="xzImg" src="https://cdn.zhoukaiwen.com/xz.png"></cover-image>

				<!-- 打开闪光灯 -->
				<cover-image @click="sgdBtn" class="sgdImg" :src="sgdUrl"></cover-image>

				<!-- 拍照 -->
				<cover-view class="cameraBtn" @click="takePhoto">
					<cover-view class="cameraBtn2"></cover-view>
				</cover-view>

				<!-- 历史 / 取消 -->
				<cover-view class="bottomBtn" v-if="imgList.length < 1">
					<cover-view @click="history" class="btn">历史</cover-view>
					<cover-view class="btn" @click="goBack">取消</cover-view>
				</cover-view>

				<!-- 删除图片 -->
				<cover-view class="bottomBg" v-if="imgList.length > 0">
					<cover-view>
						<cover-view @click="ViewImage(index)" class="imgBox" v-for="(item,index) in imgList"
							:key="index">
							<cover-image class="imgItem" :src="item.src" mode="aspectFill"></cover-image>
							<cover-view class="cu-tag" @tap.stop="DelImg" :data-index="index">
								<cover-image class="iconClose" src="https://cdn.zhoukaiwen.com/icon_close.png"
									mode="aspectFill"></cover-image>
							</cover-view>
						</cover-view>
					</cover-view>

					<!-- 上报 -->
					<cover-view @click="reportBtn" class="report">上报（{{imgList.length}}）</cover-view>
				</cover-view>


				<!-- 上报的内容 -->
				<!-- 弹窗形式 -->
				<view class="u-demo-area">

					<u-popup border-radius="10" v-model="show" :mode="mode" length="50%" :mask="mask"
						:closeable="closeable" :close-icon-pos="closeIconPos">

						<view class="u-demo-area">
							<view class="">
								<u-radio-group :shape="shape" :size="size" :width="width" :wrap="wrap" v-model="value"
									@change="radioGroupChange" :activeColor="activeColor">
									<u-radio @change="radioChange" v-for="(item, index) in message"
										:disabled="item.disabled" :key="index" :name="item.name">{{item.name}}</u-radio>
								</u-radio-group>
							</view>
							<view class="u-demo-result-line">
								{{value ? `选中了"${result}"` : '请选择'}}
							</view>
						</view>

					</u-popup>

				</view>



				<!-- 	<cover-view v-if="rreportShow" class="reportBox animation-slide-bottom">
					<cover-view class="text-lg margin-left-sm margin-bottom-sm"
						style="height:78rpx;line-height: 78rpx;">请选择照片备注内容</cover-view>
					<cover-view class="listBox">
						<cover-view class="item" v-for="(item,index) in message" :key="index" :name="item.name" @click="getMessage">
							{{item.name}}
						</cover-view>
						<cover-view class="item active">堵车</cover-view>
						<cover-view class="item">事故</cover-view>
						<cover-view class="item">停车休息</cover-view>
						<cover-view class="item">车辆故障</cover-view>
						<cover-view class="item">加油</cover-view>
						<cover-view class="item">恶劣天气</cover-view>
						<cover-view class="item">货物巡检</cover-view>
						<cover-view class="item">其他</cover-view>
					</cover-view>
					<cover-view @click="endBtn" class="repBtn">上报信息</cover-view>
				</cover-view> -->




			</camera>

			<view style="position: absolute;top: -999999px;">
				<view><canvas :style="{ width: w, height: h }" canvas-id="firstCanvas"></canvas></view>
			</view>

		</view>
	</view>
</template>

<script>
	// 导入获取地理位置插件
	import QQMapWX from "@/common/qqmap-wx-jssdk.min.js";

	export default {
		data() {
			return {

				// 弹出层信息
				show: false,
				mode: 'bottom',
				mask: true, // 是否显示遮罩
				closeable: true,
				closeIconPos: 'top-right',

				// 获取用户名称(照片上的经办人姓名)
				info: [],

				getHeight: '200',
				device: 'back', //前置或后置摄像头，值为front, back
				flash: 'off', //闪光灯，值为auto, on, off
				nowTime: '', //日期
				nowTime2: '', //时间
				address: '', //当前地址信息
				sgdUrl: 'https://cdn.zhoukaiwen.com/sgd.png', //开启闪光灯

				// 存储上传时图片的地址
				imgListData: [],
				newImgData: [],
				imgList: [],

				rreportShow: false, //选择照片备注内容弹窗

				src: [], // 图片地址
				w: '',
				h: '',

				// 图片备注的内容
				formData: {
					value: 1,
					hobby: [1]
				},


				// 单选框
				disabled: false,
				result: '荔枝',
				shape: 'circle',
				value: '荔枝',
				activeColor: 'success',
				size: 50,
				wrap: false,
				width: 'auto',
				message: [{
						name: '转让合同',
						checked: true,
						disabled: false
					},
					{
						name: '拍卖合同',
						checked: true,
						disabled: false
					},
					{
						name: '经营合同',
						checked: true,
						disabled: false
					},
					{
						name: '借款合同',
						checked: true,
						disabled: false
					},
					{
						name: '抵押合同',
						checked: true,
						disabled: false
					},
					{
						name: '定金合同',
						checked: true,
						disabled: false
					},
					{
						name: '建设工程合同',
						checked: true,
						disabled: false
					},
					{
						name: '其他',
						checked: true,
						disabled: false
					}
				],
				
				
			}
		},
		onShow() {
			// 获取姓名
			this.getInfo()
		},
		onLoad() {

			const that = this;
			var qqmapsdk;
			uni.getSystemInfo({
				success: function(res) {
					that.getHeight = res.windowHeight;
				}
			});

			// 进去后的弹窗
			uni.showModal({
				title: '拍照存档上报',
				content: '合同签署过程中，为确保流程的安全性和警惕性，请拍照上报企业，保留证据。',
				showCancel: false,
				confirmText: '知道了',
				confirmColor: '#3056F6',
				success: function(res) {
					if (res.confirm) {
						console.log('用户点击确定');
					}
				}
			});

			// 获取当前时间
			this.getTime();

			// 获取当前的地理位置
			uni.getLocation({
				type: 'wgs84',
				success: function(res) {
					console.log('当前位置的经度：' + res.longitude);
					console.log('当前位置的纬度：' + res.latitude);

					qqmapsdk = new QQMapWX({
						key: "O5IBZ-O3E3V-TJTPX-UZKMP-FOW36-IOBLL" //自己申请的key
					});
					qqmapsdk.reverseGeocoder({
						location: {
							latitude: res.latitude,
							longitude: res.longitude
						},
						success(addressRes) {
							console.log("打印获取位置的json数据===>")
							console.log(addressRes)
							that.address = addressRes.result.address;
						},
						fail(res) {}
					});
				}
			});
		},

		methods: {

			// 选中某个单选框时，由radio时触发
			radioChange(e) {
				console.log("选择了====>")
				console.log(e);
			},
			radioGroupChange(e) {
				this.result = e;
				//console.log(e);
			},

			// 获取用户姓名的数据
			async getInfo() {
				const res = await this.$myRequest({
					// url: '/goodsdetail?limit=1&page=1&sort=1&id=' + this.id
					url: '/pcuser/info?token=' + uni.getStorageSync('token')
				})
				console.log("用户信息")
				console.log(res)
				// this.info = res.data.data.items[0]
				this.info = res.data.data
				console.log(res.data.data)
			},

			// 上报：最终上报
			endBtn() {
				this.rreportShow = false;
				this.imgList = [];
				console.log("最终上报")
				console.log("经办人姓名===>" + this.info.username)
				console.log("时间===>" + this.nowTime2)
				console.log("日期===>" + this.nowTime)
				console.log("地点===>" + this.address)
				console.log("备注===>")
				console.log(this.imgList)
				this.$tip.success("上传成功")
				uni.navigateTo({
					url: './index'
				})


			},

			// 上报：选择类型
			reportBtn() {

				// 唤起弹窗
				// this.show = true;

				this.rreportShow = true;

				var str = this.imgListData;
				// const newImgData = [this.imgListData.substr(0, this.imgListData.length - 1)];
				const newImgData = this.imgListData.substr(0, this.imgListData.length - 1);

				// 实现多张图片上传到服务器

				// for(let i = 0;i < newImgData.length; i++) {

				// 	uni.uploadFile({

				// 		// url:服务器地址
				// 		url:  this.$BASE_URL.BASE_URL+'/upload',
				// 		method: 'POST',

				// 		filePath: newImgData[1],

				// 		// 获取要上传文件资源的路径
				// 		// filePath: newImgData,
				// 		//文件对应的 key , 开发者在服务器端通过这个 key 可以获取到文件二进制内容
				// 		// name: 'file',

				// 		//成功的回调
				// 		success: uploadFileRes => {
				// 			console.log("进来了")
				// 			//获取图片信息 网站域名 + res1.data.url就是一个图片的完整路径了
				// 			var res1 = JSON.parse(uploadFileRes.data);
				// 			// that.src1 = res1.data.url;
				// 			// console.log(res1.data.url)
				// 			console.log(res1)
				// 		}
				// 	})

				// }
				console.log("newImgDATA")
				console.log(newImgData)

				// // 实现单张图片上传到服务器
				uni.uploadFile({

					// url:服务器地址
					url: this.$BASE_URL.BASE_URL + '/upload',
					method: 'POST',

					// 获取要上传文件资源的路径
					filePath: newImgData,
					//文件对应的 key , 开发者在服务器端通过这个 key 可以获取到文件二进制内容
					name: 'file',

					//成功的回调
					success: uploadFileRes => {
						console.log("进来了")
						//获取图片信息 网站域名 + res1.data.url就是一个图片的完整路径了
						var res1 = JSON.parse(uploadFileRes.data);
						// that.src1 = res1.data.url;
						// console.log(res1.data.url)
						console.log(res1)
						uni.navigateTo({
							url:"./index"
						})
					}
				})
			},


			// 翻转摄像头
			xzBtn() {
				if (this.device == 'front') {
					this.device = 'back'
				} else {
					this.device = 'front'
				}
			},

			// 开启闪光灯
			sgdBtn() {
				if (this.flash == 'off') {
					this.flash = 'on'
					this.sgdUrl = 'https://cdn.zhoukaiwen.com/sgd_on.png'
				} else {
					this.flash = 'off'
					this.sgdUrl = 'https://cdn.zhoukaiwen.com/sgd.png'
				}
			},

			// 删除图片
			DelImg(e) {
				uni.showModal({
					// title: '异常照片',
					content: '确定要删除这张照片吗？',
					cancelText: '再看看',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							console.log(e)
							this.imgList.splice(e.currentTarget.dataset.index, 1)
							console.log(this.imgList)

							var arr = this.imgList;
							var str = '';
							for (var i = 0; i < arr.length; i++) {
								str += arr[i].src + ",";
							}
							this.imgListData = str;
							console.log(this.imgListData)
						}
					}
				})
			},

			// 查看照片
			ViewImage(index) {
				const imgList = [this.imgList[index].src];
				console.log("该图片的地址为==>" + imgList)
				uni.previewImage({
					urls: imgList,
				});
			},

			// 点击拍照
			takePhoto() {
				var that = this;
				// if (this.imgList.length < 3) {
					if (this.imgList.length < 1) {
					const ctx = uni.createCameraContext();
					ctx.takePhoto({
						quality: 'high',
						success: (res) => {
							var tempImagePath = res.tempImagePath;

							// 获取图片信息
							uni.getImageInfo({
								src: res.tempImagePath,
								success: ress => {
									that.w = ress.width / 3 + 'px';
									that.h = ress.height / 3.01 + 'px';
									let ctx = uni.createCanvasContext('firstCanvas'); /** 创建画布 */
									//将图片src放到cancas内，宽高为图片大小
									ctx.drawImage(res.tempImagePath, 0, 0, ress.width / 3, ress
										.height / 3);
									ctx.setFontSize(12);
									ctx.setFillStyle('#FFFFFF');
									// ctx.rotate(30 * Math.PI / 180);
									let textToWidth = (ress.width / 3) * 0.03;

									let textToHeight = (ress.height / 3) * 0.9;

									// 经办人姓名
									ctx.fillText('经办人姓名：' + this.info.username, textToWidth,
										textToHeight);

									ctx.setFontSize(10);
									let textToHeight2 = (ress.height / 3) * 0.94;

									// 拍照时间
									ctx.fillText(that.nowTime + ' ' + that.nowTime2, textToWidth,
										textToHeight2);

									let textToHeight3 = (ress.height / 3) * 0.98;

									// 拍照地点
									ctx.fillText(that.address, textToWidth, textToHeight3);

									ctx.draw(false, () => {
										setTimeout(() => {
											uni.canvasToTempFilePath({
												canvasId: 'firstCanvas',
												success: res1 => {
													tempImagePath =
														res1
														.tempFilePath;
													console.log(
														'图片地址===>',
														tempImagePath
													);
													this.imgList.push({
														src: tempImagePath
													})
													console.log(
														"imgList===>"
													)
													console.log(this
														.imgList);

													var arr = this
														.imgList
													var str = '';
													for (var i = 0; i <
														arr.length; i++
													) {
														str += arr[i]
															.src + ",";
													}
													// 将图片存入数组
													this.imgListData =
														str;
													console.log(this
														.imgListData
													)
													console.log(
														"最后图片信息为：" +
														str)
												}
											});
										}, 1000);
									});
								}
							});
						}
					});
				} else {
					uni.showToast({
						title: '最大上传1张照片',
						duration: 2000,
						icon: 'none'
					});
				}
			},

			error(e) {
				console.log(e.detail);
			},


			getTime: function() {
				var date = new Date(),
					year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '年' + month + '月' + day + '日';
				var timer2 = hour + ':' + minute + ':' + second;
				this.nowTime = timer;
				this.nowTime2 = timer2;

				console.log("当前年月日==>" + this.nowTime);
				console.log("当前时间==>" + this.nowTime2);
			},
			goBack() {
				uni.navigateBack({
					delta: 1
				});
			},
			history() {
				uni.navigateTo({
					url: 'timeline'
				})
			}
		}
	}
</script>

<style lang="scss">
	@import '@/common/uni-nvue.scss';

	.example {
		padding: 10px;
		background-color: #fff;
	}

	.text {
		font-size: 14px;
		color: #333;
		margin-bottom: 10px;
	}

	.topBox {
		width: 750rpx;
		box-sizing: border-box;
		// padding: 30rpx;
		padding: 114rpx 43rpx;
		color: #EEEEEE;
		font-size: 34rpx;

		.topItem {
			width: 100%;
			white-space: pre-wrap;
			margin-bottom: 15rpx;
		}
	}

	.cameraBtn {
		width: 120rpx;
		height: 120rpx;
		line-height: 120rpx;
		border: 6rpx #FFFFFF solid;
		border-radius: 50%;
		padding: 8rpx;
		position: absolute;
		left: calc(50% - 60rpx);
		bottom: 210rpx;
	}

	.cameraBtn2 {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		background-color: #FFFFFF;
		text-align: center;
		color: #007AFF;
	}

	.xzImg {
		width: 52rpx;
		height: auto;
		position: absolute;
		right: 44rpx;
		bottom: 580rpx;
	}

	.sgdImg {
		width: 40rpx;
		height: auto;
		position: absolute;
		right: 50rpx;
		bottom: 450rpx;
	}

	.bottomBtn {
		width: 100%;
		height: 150rpx;
		padding-bottom: 15rpx;
		position: absolute;
		bottom: 0;
		left: 0;
		text-align: center;
		display: flex;
		justify-content: space-between;

		.btn {
			width: 30%;
			height: 150rpx;
			font-size: 34rpx;
			color: #FFFFFF;
			line-height: 150rpx;
		}
	}

	.bottomBg {
		width: 100%;
		height: 170rpx;
		box-sizing: border-box;
		padding: 20rpx 30rpx 40rpx;
		position: absolute;
		bottom: 0;
		left: 0;
		background-color: rgba(0, 0, 0, .8);
		display: flex;
		justify-content: space-between;
		align-items: center;

		.imgBox {
			width: 110rpx;
			height: 110rpx;
			float: left;
			margin-right: 40rpx;
			position: relative;

			.cu-tag {
				position: absolute;
				right: 0;
				top: 0;
				border-bottom-left-radius: 2px;
				padding: 3px 5px;
				height: auto;
				background-color: rgba(0, 0, 0, 0.5);
				font-size: 10px;
				vertical-align: middle;
				font-family: Helvetica Neue, Helvetica, sans-serif;
				white-space: nowrap;
				color: #ffffff;
			}
		}

		.imgItem {
			width: 110rpx;
			height: 110rpx;
		}
	}

	.report {
		height: 68rpx;
		line-height: 68rpx;
		text-align: center;
		color: #FFFFFF;
		box-sizing: border-box;
		padding: 0rpx 20rpx;
		border-radius: 10rpx;
		background-color: #2157FF;
	}

	.iconClose {
		width: 20rpx;
		height: 20rpx;
	}

	.reportBox {
		width: 750rpx;
		height: auto;
		box-sizing: border-box;
		padding: 10rpx 20rpx;
		background-color: #FFFFFF;
		position: absolute;
		bottom: 0;
	}

	.listBox {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;

	}

	.item {
		width: 160rpx;
		height: 68rpx;
		line-height: 68rpx;
		text-align: center;
		background: #F7F7F9;
		margin-bottom: 40rpx;
		color: #888888;

	}

	.active {
		background-color: #F1F7FF;
		color: #025ADD;
	}

	.repBtn {
		width: 680rpx;
		height: 78rpx;
		line-height: 78rpx;
		background-color: #025ADD;
		color: #FFFFFF;
		font-size: 33rpx;
		text-align: center;
		border-radius: 10rpx;
		margin: 5rpx auto 20rpx;
	}
</style>
