<!-- 通讯录详情页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 12 9
-->
<template>
	<view class="u-wrap">
		<scroll-view :scroll-y="modalName==null" class="page">
		<cu-custom bgColor="bg-gradual-blue" :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">信息详情</block>
		</cu-custom>
	
			<view class="cu-list menu-avatar margin-top">
				<view class="cu-item" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
					<view class="cu-avatar round lg" v-if="infoList.avatar"  :style="[{backgroundImage:'url('+ infoList.avatar +')'}]"></view>
					<view class="cu-avatar round lg" v-else :style="[{backgroundImage:'url('+ avatar +')'}]"></view>
					<view class="content">
						<view class="text-grey text-lg">{{infoList.realname?infoList.realname:infoList.username}}<text class="cuIcon-peoplefill margin-left-sm" :class="infoList.sex=='2'?'text-pink':'text-blue'"></text></view>
						<view class="text-gray text-sm flex">
							<view class="text-cut">
								{{infoList.post}}
							</view>
						</view> 
					</view>
				</view>	
			</view>
			
			<view class="cu-list menu margin-top">
				
				<!-- 拨打电话 -->
				<view class="cu-item" @click="callPhoneNumber" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
					<view class='content'>
						<image src='../../static/calldetailIcon/call.png' class='png' mode='aspectFit'></image>
						<text class='text-lg margin-sm'>手机号</text>
					</view>
					<view class="action">
						<view class="cu-tag round bg-blue light">{{infoList.phone?infoList.phone:phone}}</view>
					</view>
				</view>
				
					<!-- <view class="cu-item animation-slide-bottom" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
						<view class="content" @click="callPhoneNumber">
							<text class="text-grey">手机</text>
						</view>
						<view class="action" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
							<text class="text-grey">{{infoList.phone?infoList.phone:phone}}</text>
						</view>
					</view> -->
					
					<view class="cu-item animation-slide-bottom" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
						<view class="content">
							<image src='../../static/calldetailIcon/email.png' class='png' mode='aspectFit'></image>
							<text class="text-grey">邮箱</text>
						</view>
						<view class="action">
							<text class="text-grey">{{infoList.email?infoList.email:email}}</text>
						</view>
					</view>
					<view class="cu-item animation-slide-bottom" :style="[{animation: 'show ' + 0.6+ 's 1'}]">
						<view class="content">
							<text class="text-grey">部门</text>
						</view>
						<view class="action">
							<text class="text-grey">{{infoList.departName}}</text>
						</view>
					</view>
					
				</view>
				
			</scroll-view>
		
		
		
	</view>
</template>

<script>

	export default {
		data() {
			return {
				id: "",
				phone: "",
				email: '',
				company: '',
				NavBarColor:this.NavBarColor,
				avatar:'',
				modalName: null,
				infoList:{
					realname:'',
					post:'何逢君',
					avatar:'https://cdn.jsdelivr.net/gh/Dorian1015/cdn/img/custom/tuxiang.jpg',
					phone: '15178672798',
					email:'lijinghailjh@163.com',
					department:'技术部',
					title:'标题'
				},
			}
		},
		onLoad (options) {
			console.log(options)
			this.id = options.id
		},
		methods: {
			//拨打固定电话
			callPhoneNumber() {
				uni.makePhoneCall({
					phoneNumber: this.infoList.phone,
				});
			},
		}
	}
</script>

<style>
	.page {
		height: 100Vh;
		width: 100vw;
	}
	
	.page.show {
		overflow: hidden;
	}
	
	.switch-sex::after {
		content: "\e716";
	}
	
	.switch-sex::before {
		content: "\e7a9";
	}
	
	.switch-music::after {
		content: "\e66a";
	}
	
	.switch-music::before {
		content: "\e6db";
	}
</style>
