<!-- 查询页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 4
-->
<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">查询页面</block>
		</cu-custom>
		<view class="padding flex flex-wrap justify-between align-center bg-white">
			<button class="cu-btn round" @click="myCliack">圆角</button>
		</view>
	</view>
</template>

<script>
	export default {
		data(){
			return{
				
			}
		},
		onUnload() {
			uni.switchTab({
				url:'../index/index'
			})
		},
		methods: {
			myCliack(){
				//当前页
				var pages = getCurrentPages();
				//上个页面
				var beforePage = pages[pages.length - 2];
				beforePage.$vm.NumSteps(3)
			}
		}
	}
</script>

<style>
</style>
