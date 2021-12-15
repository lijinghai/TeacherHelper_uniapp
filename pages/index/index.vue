<template>
	<view>
		<scroll-view :scroll-y="modalName==null" class="page" :class="modalName!=null?'show':''">
			<cu-custom bgColor="bg-gradual-blue" :isBack="false">
				<!-- <block slot="backText">返回</block> -->
				<block slot="content">首页</block>
			</cu-custom>
			<add-tip :tip="tip" :duration="duration" />

			<view class="wrap">
				<view class="search">
					<u-search v-model="keywords" @custom="search" @search="search"></u-search>
				</view>
			</view>

			<view class="cu-list menu-avatar">
				<!-- 公告通知 -->
				<view class="cu-item" @click="navTo('/pages/msg/list-item')">
					<view class="cu-avatar round lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg);">
					</view>
					<view class="content">
						<view class="text-grey">公告通知</view>
						<view class="text-gray text-sm flex">
							<view class="text-cut">
								<text class="cuIcon-infofill text-red  margin-right-xs"></text>
								关于元旦放假的通知
							</view>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cu-tag round bg-grey sm">5</view>
					</view>
				</view>
				<!-- 新闻动态 -->
				<view class="cu-item" @click="navTo('/pages/msg/list-item')">
					<view class="cu-avatar round lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Taric.png);">
						<view class="cu-tag badge">99+</view>
					</view>
					<view class="content">
						<view class="text-grey">
							<view class="text-cut">新闻动态</view>
							<view class="cu-tag round bg-orange sm">新闻</view>
						</view>
						<view class="text-gray text-sm flex">
							<view class="text-cut">
								神十三航天员圆满完成出舱任务
							</view>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cuIcon-notice_forbid_fill text-gray"></view>
					</view>
				</view>
				<!-- 日常办公 -->
				<view class="cu-item " @click="navTo('/pages/msg/examine-item')">
					<view class="cu-avatar radius lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Morgana.png);">
					</view>
					<view class="content">
						<view class="text-pink">
							<view class="text-cut">日常办公</view>
						</view>
						<view class="text-gray text-sm flex">
							<view class="text-cut">何逢君提交的“请假申请”待你审批</view>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cu-tag round bg-red sm">5</view>
					</view>
				</view>
				<!--网上报销  -->
				<view class="cu-item grayscale" @click="navTo('/pages/msg/list-item')">
					<view class="cu-avatar radius lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big81007.jpg);">
					</view>
					<view class="content">
						<view>
							<view class="text-cut">网上报销</view>
							<view class="cu-tag round bg-orange sm">等待报销...</view>
						</view>
						<view class="text-gray text-sm flex">
							<view class="text-cut">吴天祥提交的“差旅费报销”待你审批</view>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cu-tag round bg-red sm">5</view>
					</view>
				</view>
				<!-- 我的邮件 -->
				<view class="cu-item cur" @click="navTo('/pages/msg/list-item')">
					<view class="cu-avatar radius lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big81020.jpg);">
						<view class="cu-tag badge"></view>
					</view>
					<view class="content">
						<view>
							<view class="text-cut">我的邮件</view>
							<view class="cu-tag round bg-orange sm">6人</view>
						</view>
						<view class="text-gray text-sm flex">
							<view class="text-cut"> 邮件提醒：<text
									class="cuIcon-locationfill text-orange margin-right-xs"></text> 邮件提醒：15封新邮件未读</view>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cuIcon-notice_forbid_fill text-gray"></view>
					</view>
				</view>
			</view>


		</scroll-view>
	</view>
</template>

<script>
	import addTip from '../../components/wxcomponents/struggler-uniapp-add-tip/struggler-uniapp-add-tip.vue';
	export default {
		components: {
			addTip
		},
		data() {
			return {
				tip: '点击「添加小程序」，下次访问更便捷',
				modalName: null,
				keywords: ''
			};
		},
		methods: {
			navTo(url) {
				uni.navigateTo({
					url: url
				});
			},
			search(value) {
				this.$u.toast('搜索内容为：' + value)
			}

		}
	}
</script>

<style lang="scss">
	@import '@/common/uni2.css';

	.page {
		height: 100Vh;
		width: 100vw;
	}

	.wrap .search {
		background: #ffffff;
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
