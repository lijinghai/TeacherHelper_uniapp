<!-- 通讯录详情页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 12 9
-->
<template>
	<view class="u-wrap">
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">信息详情</block>
		</cu-custom>
		
		<view>
			{{id}}
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0
			}
		},
		methods: {
			onLoad (options) {
				console.log(options)
				this.id = options.id
			}
		}
	}
</script>

<style>
</style>
