<!-- 完成
页页面
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 8 12
 -->
<template>
	<view>
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="false">
				<!-- <block slot="backText">返回</block> -->
				<block slot="content">认证完成</block>
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

		<view class="solids-bottom padding-xs flex align-center">
			<view class="flex-sub text-center">
				<view class="solid-bottom text-xsl padding">
					<text class=" cuIcon-roundcheckfill text-green"></text>
				</view>
				<!-- <view class="padding">{{item.msg}}</view> -->
				<view class="padding">{{item.msg}}</view>
			</view>
		</view>
		<view class="padding flex flex-direction">
			<button class="cu-btn bg-green shadow-blur round lg" @tap="goback()">注册完成
			</button>
		</view>



	</view>
</template>

<script>
	export default {
		data() {
			return {

				modalName: null,
				item: {
					msg: '注册完成'
				},

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
		onLoad() {
			this.NumSteps(4)
		},
		// 返回页面跳转
		onUnload() {
			uni.navigateTo({
				url: './personal'
			})
		},
		methods: {

			// 完成
			goback() {
				uni.switchTab({
					url: '../index/index'
				})
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

<style>



</style>
