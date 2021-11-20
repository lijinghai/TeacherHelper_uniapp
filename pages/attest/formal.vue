<!-- 绑定正式章页面
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 8 11
 -->
<template>
	<view>
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="true">
				<block slot="backText">返回</block>
				<block slot="content">绑定正式章</block>
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
		
		<view class="padding bg-white">
			<view class="text-left padding">*请选择主体类型:</view>
			<view class="text-center padding">我于杀戮之中绽放 ,亦如黎明中的花朵</view>
			<view class="text-right padding text-blue" @click="goNext">下一步</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
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
			this.NumSteps(2)
		},
		// 返回页面跳转
		onUnload() {
			uni.navigateTo({
				url:'./index'
			})
		},
		methods: {
			
			// 跳转到下一页面
			goNext() {
				uni.navigateTo({
					url:'./personal'
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
				console.log("step======>"+step)
				if (step != null ) {
					this.num = step-1
					console.log('如果正处于当前第'+step+'步骤,则使用这个图片===>'+step)
				} 
				else if (step > this.num) {
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
