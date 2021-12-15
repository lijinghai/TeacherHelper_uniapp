<!-- 通讯录页面
  @author: lijing
  @email: lijinghailjh@163.com
  @Date: 2021 12 9
-->
<template>
	<view class="u-wrap">
		<cu-custom bgColor="bg-gradual-blue" :isBack="false">
			<!-- <block slot="backText">返回</block> -->
			<block slot="content">通讯录</block>
		</cu-custom>
		
	<view class="cu-bar bg-white search fixed" :style="[{top:CustomBar + 'px'}]">
		<view class="search-form round">
			<text class="cuIcon-search"></text>
			<input type="text" v-model="keywords" placeholder="输入搜索的关键词" confirm-type="search" @confirm="searchUserByKey"></input>
			<!-- <u-search v-model="keywords" @custom="search" @search="search"></u-search> -->
		</view>
		<view class="action">
			<button class="cu-btn bg-gradual-blue shadow-blur round" @tap="searchUserByKey">搜索</button>
		</view>
	</view>
		
		<view class="u-search-box">
			<view class="u-search-inner">
				<u-icon name="search" color="#909399" :size="28"></u-icon>
				<text class="u-search-text">搜索您想查找的学生姓名</text>
			</view>
		</view>
		
		
		<view class="u-menu-wrap">
			<scroll-view scroll-y scroll-with-animation class="u-tab-view menu-scroll-view" :scroll-top="scrollTop">
				<view v-for="(item,index) in tabbar" :key="index" class="u-tab-item" :class="[current==index ? 'u-tab-item-active' : '']"
				 :data-current="index" @tap.stop="swichMenu(index)">
					<text class="u-line-1">{{item.name}}</text>
				</view>
			</scroll-view>
			<block v-for="(item,index) in tabbar" :key="index">
				<scroll-view scroll-y class="right-box" v-if="current==index">
					<view class="page-view">
						<view class="class-item">
							<view class="item-title">
								<text>{{item.name}}</text>
							</view>
							<view class="item-container">
								<view class="thumb-box" v-for="(item1, index1) in item.foods" :key="index1">
									<image @click="previewImg(item1.id)" class="item-menu-image" :src="item1.icon" mode=""></image>
									<view class="item-menu-name margin-top-sm">{{item1.name}}</view>
								</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</block>
		</view>
	</view>
</template>

<script>
	import classifyData from "@/common/classify.data.js";
	export default {
		data() {
			return {
				CustomBar: this.CustomBar,
				tabbar: classifyData,
				scrollTop: 0, //tab标题的滚动条位置
				current: 0, // 预设当前项的值
				menuHeight: 0, // 左边菜单的高度
				menuItemHeight: 0, // 左边菜单item的高度
				keywords: ''
			}
		},
		computed: {
			
		},
		methods: {
			// 搜索
			searchUserByKey(value){
				// this.loadInfo()
				this.$u.toast('搜索内容为：' + value)
			},
			loadInfo(){
				// this.$http.get(this.queryUserByKeyWord,{params:{'keyword':this.keyword}}).then(res=>{
			 //         if (res.data.success) {
				// 		console.log("res",res)
				// 	    let arr=res.data.result;
				// 		let szuArr=[];
				// 	    this.userList = arr.map(item => {
				// 	        let { id,realname,avatar,username,phone,email,post,orgCode} = item
				// 			let pinYin = username.toUpperCase();
				// 			if(realname){
				// 				//TODO 判断汉字的位置
				// 				if(/.*[\u4e00-\u9fa5]+.*$/.test(realname)){
				// 					pinYin=vPinyin.chineseToPinYin(realname);
				// 				}
				// 			}
				// 			if(avatar){
				// 				avatar=getFileAccessHttpUrl(avatar);
				// 			}
				// 	        let event = {
				// 	          id, realname ,avatar,username,phone,email,post,orgCode,
				// 			  szm:pinYin.substr(0,1)
				// 	        }
				// 			szuArr.push(event.szm)
				// 	        return event
				// 	      })
						 
				// 		  this.list=this.list.filter(item=>szuArr.indexOf(item.name)!=-1)	
				// 	      //this.list.unshift({name:"#"})
				// 	}
				// }).catch(err => {
				// 	console.log(err);
				// });
				
			},
			// 点击图标跳转
			previewImg(current) {
				console.log("点击了")
				console.log(current)
				uni.navigateTo({
					url: '/pages/call/callDetail?id='+current
				})
			},
			getImg() {
				return Math.floor(Math.random() * 35);
			},
			// 点击左边的栏目切换
			async swichMenu(index) {
				if(index == this.current) return ;
				this.current = index;
				// 如果为0，意味着尚未初始化
				if(this.menuHeight == 0 || this.menuItemHeight == 0) {
					await this.getElRect('menu-scroll-view', 'menuHeight');
					await this.getElRect('u-tab-item', 'menuItemHeight');
				}
				// 将菜单菜单活动item垂直居中
				this.scrollTop = index * this.menuItemHeight + this.menuItemHeight / 2 - this.menuHeight / 2;
			},
			// 获取一个目标元素的高度
			getElRect(elClass, dataVal) {
				new Promise((resolve, reject) => {
					const query = uni.createSelectorQuery().in(this);
					query.select('.' + elClass).fields({size: true},res => {
						// 如果节点尚未生成，res值为null，循环调用执行
						if(!res) {
							setTimeout(() => {
								this.getElRect(elClass);
							}, 10);
							return ;
						}
						this[dataVal] = res.height;
					}).exec();
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.u-wrap {
		height: calc(100vh);
		/* #ifdef H5 */
		height: calc(100vh - var(--window-top));
		/* #endif */
		display: flex;
		flex-direction: column;
	}

	.u-search-box {
		padding: 18rpx 30rpx;
	}

	.u-menu-wrap {
		flex: 1;
		display: flex;
		overflow: hidden;
	}

	.u-search-inner {
		background-color: rgb(234, 234, 234);
		border-radius: 100rpx;
		display: flex;
		align-items: center;
		padding: 10rpx 16rpx;
	}

	.u-search-text {
		font-size: 26rpx;
		color: $u-tips-color;
		margin-left: 10rpx;
	}

	.u-tab-view {
		width: 200rpx;
		height: 100%;
	}

	.u-tab-item {
		height: 110rpx;
		background: #f6f6f6;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 26rpx;
		color: #444;
		font-weight: 400;
		line-height: 1;
	}
	
	.u-tab-item-active {
		position: relative;
		color: #000;
		font-size: 30rpx;
		font-weight: 600;
		background: #fff;
	}
	
	.u-tab-item-active::before {
		content: "";
		position: absolute;
		border-left: 4px solid $u-type-primary;
		height: 32rpx;
		left: 0;
		top: 39rpx;
	}

	.u-tab-view {
		height: 100%;
	}
	
	.right-box {
		background-color: rgb(250, 250, 250);
	}
	
	.page-view {
		padding: 16rpx;
	}
	
	.class-item {
		margin-bottom: 30rpx;
		background-color: #fff;
		padding: 16rpx;
		border-radius: 8rpx;
	}
	
	.item-title {
		font-size: 30rpx;
		color: $u-main-color;
		font-weight: bold;
		margin: 10rpx 0;
	}
	
	.item-menu-name {
		font-weight: normal;
		font-size: 24rpx;
		color: $u-main-color;
	}
	
	.item-container {
		display: flex;
		flex-wrap: wrap;
	}
	
	.thumb-box {
		width: 33.333333%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		margin-top: 20rpx;
		margin-bottom: 10rpx;
	}
	
	.item-menu-image {
		width: 120rpx;
		height: 120rpx;
		border-radius: 100rpx;
	}
</style>
