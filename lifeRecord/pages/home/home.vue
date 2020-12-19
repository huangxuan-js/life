<template>
	<view class="content">
		<view class="" style="height: 400rpx;">
			<view class="" style="width: 100%;height: 360rpx; background:linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.1)), url(https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-wwgjizeoojbef6e56c/24d77650-41d2-11eb-b997-9918a5dda011.jpg);background-size:100% 100%;">

			</view>
			<view class="u-flex u-row-center ">
				<u-image width="34rpx" height="34rpx" mode="scaleToFill" :src="iconBook"></u-image>
				<view class="" style="width: 600rpx;">
					<u-notice-bar mode="vertical" :volume-icon="false" :more-icon="true" type="none" :list="list"></u-notice-bar>
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
				iconBook:require('../../static/icons/book.png')
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style lang="scss">

</style>
