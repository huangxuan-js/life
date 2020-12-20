<template>
	<view>
		<view class="u-relative" style="height: 400rpx;">
			<view class="top-wpr">
				<view class="u-flex u-row-right u-p-t-40 u-p-r-30">
					<u-icon name="search" color="#000000" size="50" class="u-m-r-40"></u-icon>
					<u-icon name="more-dot-fill" color="#000000" size="50" style="transform: rotate(90deg);"></u-icon>
				</view>
				<view class="u-flex u-row-between u-p-l-30 u-p-r-30">
					<view class="">
						<view style="font-weight: 700;font-size: 90rpx;font-family: KaiTi;">
							43
						</view>
						<view class="">
							时长领书卷立即兑换
						</view>
					</view>
					<view class="">
						领50书卷
					</view>
				</view>
			</view>
			<view class="rotation u-flex u-row-center">
				<view class="u-flex rotation-content">
					<u-image width="40rpx" height="40rpx" mode="scaleToFill" :src="iconBook" style="transform: translate(24rpx,0);"></u-image>
					<view class="u-flex-1">
						<u-notice-bar mode="vertical" :volume-icon="false" :more-icon="true" type="none" :list="list"></u-notice-bar>
					</view>
				</view>
			</view>
		</view>
		<view class="u-m-t-20 u-p-l-30 u-p-r-30 ">
			<view class="book-item u-relative u-flex u-m-b-30" v-for="(item,index) in 8" :key="index">
				<u-image width="106rpx" height="130rpx" :src="url" class="u-m-r-20" style="border-radius: 8rpx;overflow: hidden;"></u-image>
				<view class="">
					<view class="u-m-b-20">
						长夜余火
					</view>
					<view class="book-describe ">
						更新至第569话  正确的选择
					</view>
				</view>
				<button class="u-reset-button u-font-12 icon-update" v-show="false">更新</button>
				<view class="u-flex-1 u-flex u-row-right">
					<u-checkbox-group >
						<u-checkbox v-model="checked" shape="circle"></u-checkbox>
					</u-checkbox-group>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [
					'寒雨连江夜入吴',
					'平明送客楚山孤',
					'洛阳亲友如相问',
					'一片冰心在玉壶'
				],
				iconBook: require('../../static/icons/book.png'),
				url: "https://ggzqimg.cdn.bcebos.com/BookFiles/BookImages/zhangyeyuhuo.jpg",
				checked: true,
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.top-wpr {
		width: 100%;
		height: 360rpx;
		background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3)), url(https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-wwgjizeoojbef6e56c/24d77650-41d2-11eb-b997-9918a5dda011.jpg);
		background-size: 100% 100%;
		border-bottom-right-radius: 600rpx 60rpx;
		border-bottom-left-radius: 600rpx 60rpx;
	}

	.rotation {
		position: absolute;
		left: 0;
		bottom: 14rpx;
		width: 100%;

		.rotation-content {
			box-shadow: 0 15px 10px -12px #ccc;
			border: 1rpx solid #c8c7cc;
			width: calc(100% - 60rpx);
			border-radius: 14rpx;
			background-color: #ffffff;
		}
	}

	.icon-update {
		position: absolute;
		padding: 5rpx 14rpx;
		transform: scale(.8);
		right: 0;
		top: 0;
		background-color: #fa3534;
		color: #ffffff;
	}

	.book-describe {
		color: $u-type-info;
	}
</style>
