<!-- 关于作者页面
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 7 30
 -->
<template>
	<view class="about">
		<cu-custom :isBack="true">
			<!-- <block slot="content">注册</block> -->
			<block slot="backText">返回</block>
		</cu-custom>
		<canvas canvas-id="bubble" :style="'width:' + width + 'px;height:' + height + 'px'" class="like-fx"></canvas>
		<like-fx ref="likeFx" :width="width" :height="height"></like-fx>

		<view class="titleZ text-center align-center">
			<text class="text-bold">关于作者</text>
			<view class="contentZ">
				<text class="text-xl">项目作者:<text class="text-bold">何逢君(李京海)</text>，一名大二(2021 6 1)在读的全栈程序员，坐标南京，热爱编程，追求开源</text>
			</view>
		</view>
		
		<view class="titleZ text-center align-center margin-top-xl">
			<text class="text-bold">我希望</text>
			<view class="contentZ margin-top-lg">
				<text class="text-xl text-bold">「每一个」</text>
				<text class="text-xl">：使用我软件的用户，都可以像孩子在海边拾贝时一样的快乐。</text>
			</view>
			
		</view>

		<view class="titleZ text-center align-center margin-top-xl">
			<text class="text-bold">联系方式</text>
			<view class="contentZ margin-top-lg">
				<text class="text-xl text-bold">「邮箱」</text>
				<text class="text-xl">：lijinghailjh@163.com</text>
			</view>
			<view class="contentZ" style="margin-top: 40rpx;">
				<text class="text-xl text-bold">Github</text>
				<text class="text-xl">：https://github.com/lijinghailjh</text>
			</view>
		<!-- 	<view class="contentZ" style="margin-top: 40rpx;">
				<text class="text-xl text-bold">「Gitee」</text>
				<text class="text-xl">：https://gitee.com/Dorian15</text>
			</view> -->
		</view>

		<button class='' open-type="contact">
			<image src='../../static/touxiang.jpg' class='share-img png round shadow-lg bg-white' mode='aspectFit'>
			</image>
		</button>
	</view>
</template>

<script>
	import LikeFx from '@/components/likeFx/likeFx.vue'
	export default {
		components: {
			LikeFx
		},
		data() {
			return {
				animation_timer: null, // 动画定时器
				width: 375,
				height: 1920
			}
		},
		onLoad() {
			this._startLikeAnimation();
		},
		onShareAppMessage() {
			return {
				title: '快来和我们玩转软件开发吧～',
				path: '/pages/about/about?from=share'
			}
		},
		methods: {
			_startLikeAnimation() {
				this.animation_timer = setInterval(() => {
					this.$refs.likeFx.likeClick()
				}, 300)
			}
		}
	}
</script>

<style scoped>
	.titleZ {
		width: 750rpx;
		font-size: 52rpx;
		margin-top: 60rpx;
	}

	.contentZ {
		width: 650rpx;
		margin: 10rpx auto 0;
		text-align: left;
	}

	.about-bg {
		background-size: cover;
		width: 100vw;
		height: 100vh;
		justify-content: center;
		flex-direction: column;
		color: #fff;
	}

	.edit-fixed {
		position: fixed;
		width: 100%;
		bottom: 0;
		z-index: 1024;
		box-shadow: 0 1rpx 6rpx rgba(0, 0, 0, 0.1);
	}

	.detail-imgs image {
		width: 100%;
		float: left;
		/* height:400rpx; 不定高了*/
		border: 0;
		padding: 0;
		margin: 0
	}

	.share-img {
		position: fixed;
		padding: 10rpx;
		width: 100rpx;
		height: 100rpx;
		/* top: 680rpx; */
		bottom: 200rpx;
		right: 20rpx;
		z-index: 1024;
		opacity: 0.8;
		box-shadow: 0rpx 8rpx 30rpx 0rpx rgba(0, 0, 0, 0.3);
		border: none;
	}

	.about {
		margin: 0;
		width: 100%;
		height: 100vh;
		padding-top: 20%;
		color: #fff;
		background: linear-gradient(-120deg, #F15BB5, #9A5CE5, #01BEFF, #00F5D4);
		/* background: linear-gradient(-120deg, #0976ea, #c471f5, #f956b6, #ea7e0a); */
		background-size: 500% 500%;
		animation: gradientBG 15s ease infinite;
	}

	@keyframes gradientBG {
		0% {
			background-position: 0% 50%;
		}

		50% {
			background-position: 100% 50%;
		}

		100% {
			background-position: 0% 50%;
		}
	}

	.container {
		width: 100%;
		position: absolute;
		text-align: center;
	}

	.like-fx {
		position: fixed;
		right: 0;
		z-index: 1024;
		pointer-events: none;
		/* background-color: red; */
	}
</style>
