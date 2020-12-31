<template>
	<view>
		<view class="container u-font-16" style="height:calc(100vh - 100rpx);">
			<!-- <view class="u-m-t-20 u-m-b-20" v-for="(item,index) in textList" :key="index">
				{{item}}
			</view> -->
			<view class="uni-bg-red " style="overflow: visible; height: 100%;columns: calc(100vw - 32px) 1;column-gap: 16px;transform: translateX(-0rpx);">
				<view class="u-m-t-20 u-m-b-20" v-for="(item,index) in textList" :key="index">
					{{item}}
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				count: 1,
				textList: [],
				background: ['color1', 'color2', 'color3'],
				interval: 2000,
				duration: 500
			}
		},
		onLoad(data) {
			uni.request({
				url: 'http://api.pingcc.cn/fictionContent/search/' + data.chapterId,
				success: (res) => {
					console.log(res.data)
					this.textList = res.data.data.data[0].content
				}
			});
		},
		methods: {

		}
	}
</script>

<style>

</style>
