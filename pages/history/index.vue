<!-- 历史记录页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 10
-->
<template>
	<view>
		
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="true">
				<block slot="backText">返回</block>
				<block slot="content">历史记录页面</block>
			</cu-custom>
		</view>
		
		<view>
			<getCodeItem @itemClick="goDetail" :list="list"></getCodeItem>
		</view>
		
		<view class="padding flex flex-wrap justify-between align-center bg-white">
			<button class="cu-btn round" @click="myCliack1">提交</button>
		</view>
		
		<view class="padding flex flex-wrap justify-between align-center bg-white">
			<button class="cu-btn round" @click="myCliack2">拍照</button>
		</view>

	</view>
</template>

<script>
	import getCodeItem from '../../components/getCodeItem/index.vue';
	export default {
		data() {
			return {
				// 列表信息
				list:[
					{	
						// id
						id:'1',
						// 验证码
						"code":'1234 5678 9000',
						// 盖章单位
						"company":'南京军理股份有限公司',
						// 盖章经办人
						"handler":'李四',
						// 盖章地点
						"address": '南京秦淮',
						// 盖章时间
						"createTime": '2021年 8月 9日',
						// 图片
						"img": 'https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg'
					}
				]
			}
		},
		onUnload() {
			uni.switchTab({
				url: '../index/index'
			})
		},
		// 注册组件
		components: {
			"getCodeItem": getCodeItem
		},
		methods: {


		// 点击跳转
		goDetail(id) {
			console.log("id：" + id)
			uni.navigateTo({
				url: '/pages/getCode/edit?id=' + id
			})
		},

			// 点亮下一步 
			myCliack1() {
				//当前页
				var pages = getCurrentPages();
				//上个页面
				var beforePage = pages[pages.length - 2];
				beforePage.$vm.NumSteps(2)

			},

			// 拍照
			myCliack2() {
				uni.navigateTo({
					url: './company'
				})
			},


		}
	}
</script>

<style scoped lang="scss">

</style>
