<!-- 编辑验证码组件
@author: lijing
@email: lijinghailjh@163.com
@Date: 2021 8 10
 -->
<template>
	<view>
		<view class="cu-card article ">
			<view class="cu-item shadow borderBottom" @click="navigator(item.id)" v-for="(item,index) in list"
				:key="index">
				<view class="title">
					<view class="text-cut" :selectable="true">验证码:{{item.code}}</view>
				</view>
				<view class="content">
					<image :src="item.img" mode="aspectFit"></image>
					<view class="desc">
						<view class="text-content">
							<view>盖章单位:{{item.company}}</view>
							<view>盖章经办人:{{item.handler}}</view>
							<view>盖章地点:{{item.address}}</view>
						</view>
						<!-- <view class="text-cut">盖章经办人：XXXX</view> -->
						<view class="margin-top-xs">
							<view class="text-gray light sm round fl">盖章时间:{{item.createTime}}</view>
							<!-- <view class="text-gray light sm round fr">
								<text class="text-gray cuIcon-mark" style="font-size: 34rpx;"></text>
								<text>11</text>
							</view> -->
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		// 接收外界传来的数据
		props: ['list'],
		// 增加时间过滤器
		filters: {
			formatDate(date) {
				if (date != null) {
					console.log(date)
					const nDate = new Date(date)
					console.log(nDate)
					const year = nDate.getFullYear().toString().padStart(2, 0)
					const month = nDate.getMonth().toString().padStart(2, 0)
					const day = nDate.getDay().toString().padStart(2, 0)
					const hours = nDate.getHours().toString().padStart(2, 0)
					const minutes = nDate.getMinutes().toString().padStart(2, 0)
					return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes
				} else {
					return '（请点击填写数据）'
				}
			}
		},
		methods: {
			navigator(id) {
				// 调用父组件的方法
				this.$emit('itemClick', id)
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
