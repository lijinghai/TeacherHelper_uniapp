<!-- 身份选择页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 4
-->
<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue">
			<block slot="content">选择身份</block>
		</cu-custom>

		<view>

			<view class="padding bg-white">
				<view class="text-right ">
					<text style="margin-right: 10px;">Dorian</text>
					<text @click="goOut" style="color: #007AFF;">退出登录</text>
				</view>
			</view>

			<!-- 提示语 -->
			<view class="cu-bar ">
				<view class="action sub-title">
					<text class="text-xl text-bold text-blue">请选择当前身份</text>
					<text class="text-ABC text-blue">Select</text>
				</view>
			</view>

			<!-- 提示语小字 -->
			<view class="action">
				<text class="cuIcon-title text-green"></text>
				<text>进入后也可在【个人中心】切换其他身份</text>
			</view>

			<!-- 卡片 -->
			<view class="cu-card article ">
				<view class="cu-item shadow borderBottom" @click="navigator" v-for="(item,index) in list" :key="index">

					<view class="content">
						<image :src="item.img" mode="aspectFit"></image>
						<view class="title">
							<view class="text-cut" style="margin-top: 40rpx;">{{item.name}}</view>
						</view>
					</view>
				</view>
			</view>

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 列表信息
				list: [{
						// id
						id: '1',
						// 图片
						"img": 'https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg',
						// 身份
						"name": '我是普通用户'
					},
					{
						// id
						id: '2',
						// 图片
						"img": 'https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg',
						// 身份
						"name": '我是企业'
					}
				]

			}
		},
		methods: {
			goOut() {
				console.log("点击了")
				
				// uni.navigateTo({
				// 	url: '/pages/login/login_v2'
				// })
				
				
				
				uni.showModal({
					content: '退出后将享受不到我们的服务，确定退出吗',
					cancelText: '再看看',
					confirmText: '确认',
					success: res => {
						
						if (res.confirm) {
							// console.log(e)
							uni.navigateTo({
								url: '/pages/login/login_v2'
							})
				// 			this.imgList.splice(e.currentTarget.dataset.index, 1)
				// 			console.log(this.imgList)
				
				// 			var arr = this.imgList;
				// 			var str = '';
				// 			for (var i = 0; i < arr.length; i++) {
				// 				str += arr[i].src + ",";
				// 			}
				// 			this.imgListData = str;
				// 			console.log(this.imgListData)
						}
					}
				})
				
			}
		}
	}
</script>

<style>
	.box {
		margin: 20upx 0;
	}

	.box view.cu-bar {
		margin-top: 20upx;
	}
</style>
