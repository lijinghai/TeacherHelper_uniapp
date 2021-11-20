<!-- 获取验证码页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 8 4
-->
<template>
	<view>
		<view>
			<cu-custom bgColor="bg-gradual-blue" :isBack="true">
				<block slot="backText">返回</block>
				<block slot="content">获取验证码记录页面</block>
			</cu-custom>
		</view>
		
		<view>
			<scroll-view scroll-y class="page">
				<image src="/static/iu.jpg" mode="widthFix" class="response"></image>
				<view class="nav-list">
					<navigator hover-class="none" :url="'/pages/getCode/' + item.name" class="nav-li" navigateTo :class="'bg-'+item.color"
					 :style="[{animation: 'show ' + ((index+1)*0.2+1) + 's 1'}]" v-for="(item,index) in elements" :key="index">
						<view class="nav-title">{{item.title}}</view>
						<view class="nav-name">{{item.desc}}</view>
						<text :class="'cuIcon-' + item.cuIcon"></text>
					</navigator>
				</view>
				<view class="cu-tabbar-height"></view>
			</scroll-view>
		</view>

		<!-- <view>
			<getCodeItem @itemClick="goDetail" :list="list"></getCodeItem>
		</view> -->

		<!-- <view class="cu-card article ">
			<view class="cu-item shadow borderBottom" @click="goEdit(item.id)" v-for="(item,index) in list" :key="index">
				<view class="title">
					<view class="text-cut">验证码:{{item.code}}</view>
				</view>
				<view class="content">
					<image :src="item.img" mode="aspectFit"></image>
					<view class="desc">
						<view class="text-content">
							<view>盖章单位:{{item.company}}</view>
							<view>盖章经办人:{{item.handler}}</view>
							<view>盖章地点:{{item.address}}</view>
						</view> -->
						<!-- <view class="text-cut">盖章经办人：XXXX</view> -->
						<!-- <view class="margin-top-xs">
							<view class="text-gray light sm round fl">盖章时间:{{item.createTime}}</view> -->
							<!-- <view class="text-gray light sm round fr">
								<text class="text-gray cuIcon-mark" style="font-size: 34rpx;"></text>
								<text>11</text>
							</view> -->
	<!-- 					</view>
					</view>
				</view>
			</view>
		</view> -->

		
		
		<view class="padding flex flex-wrap justify-between align-center bg-white">
			<button class="cu-btn round" @click="myCliack">获取验证码</button>
		</view>

		<view class="box">
			<view class="cu-bar tabbar margin-bottom-xl bg-white">
				<!-- <view class="action text-green">
					<view class="cuIcon-homefill" @click="goOnly"></view> 首页
				</view> -->
			<!-- 	<view class="action text-gray">
					<view class="cuIcon-similar"></view> 分类
				</view> -->
				<view class="action text-gray add-action"  @tap="scan">
					<button class="cu-btn cuIcon-scan bg-green shadow"></button>
					扫一扫
				</view>
				<!-- 	<view class="action text-gray">
					<view class="cuIcon-cart">
						<view class="cu-tag badge">99</view>
					</view>
					购物车
				</view>
				<view class="action text-gray">
					<view class="cuIcon-my">
						<view class="cu-tag badge"></view>
					</view>
					我的
				</view> -->
			</view>
		</view>

	</view>
</template>

<script>
	// import getCodeItem from '../../components/getCodeItem/index.vue';
	export default {
		data(){
			return{
				
				elements: [
					{
						title: '扫一扫',
						desc:'扫描获取验证码',
						name: 'getScan',
						color: 'orange',
						cuIcon: 'scan',
						auth: 'ac'
					},
					{
						title: '系列码填写',
						desc:'填系列码获取验证码',
						name: 'getNum',
						color: 'cyan',
						cuIcon: 'newsfill',
						auth: 'ac'
					}
				]
				
				// 列表信息
				// list:[
				// 	{	
				// 		// id
				// 		id:'1',
				// 		// 验证码
				// 		"code":'1234 5678 9000',
				// 		// 盖章单位
				// 		"company":'南京军理股份有限公司',
				// 		// 盖章经办人
				// 		"handler":'李四',
				// 		// 盖章地点
				// 		"address": '南京秦淮',
				// 		// 盖章时间
				// 		"createTime": '2021年 8月 9日',
				// 		// 图片
				// 		"img": 'https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg'
				// 	}
				// ]
				
			}
		},
		// 返回页面跳转
		onUnload() {
			uni.switchTab({
				url:'../index/index'
			})
		},
		// 注册组件
		components: {
			// "getCodeItem": getCodeItem
		},
		methods: {
			
			// 点击跳转
			goDetail(id) {
				console.log("id：" + id)
				uni.navigateTo({
					url: '/pages/getCode/edit?id=' + id
				})
			},
			
			// 扫码
			scan(){
				 console.log("进来了")
				// #ifndef H5
				uni.scanCode({
				    success: function (res) {
						console.log('条码res：' + res);
				        console.log('条码类型：' + res.scanType);
				        console.log('条码内容：' + res.result);
						uni.navigateTo({
							url:"./"+res.result
						})
				    }
				});
				// #endif
				// #ifdef H5
				this.$tip.alert("暂不支持")
				// #endif
			},


			// 获取验证码界面
			myCliack() {
				//当前页
				var pages = getCurrentPages();
				//上个页面
				var beforePage = pages[pages.length - 2];
				beforePage.$vm.NumSteps(1)
				
				console.log("点击了")
				uni.navigateTo({
					url:"./getNum"
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.box {
		margin: 20upx 0;
		width: 100%;
		position: fixed;
		bottom: -16px;

		.margin-bottom-xl {
			margin-bottom: 20rpx;
		}
	}
</style>
