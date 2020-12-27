<template>
	<view>
		<view class="header">
			<u-navbar back-text="返回"></u-navbar>
			<view class="wrapper">
				<view class="main">
					<view class="u-flex" >
						<image src="https://bookcover.yuewen.com/qdbimg/349573/1018027842/180" style="width: 150rpx;height: 180rpx;border-radius: 8rpx;" mode="scaleToFill"></image>
						<view class="content">
								<view class="">
									万族之劫
								</view>
								<view class="">
									老鹰吃小鸡
								</view>
								<view class="">
									都市小说
								</view>
						</view>
					</view>
					<view class="container">
						<view class="">
							我是这诸天万族的劫！已有完本作品《全球高武》《重生之财源滚滚》，没看过的书友可以去看看，新书收藏一下慢慢养。
						</view>
						<view class="u-flex u-row-between">
							<view class="">
								查看目录
							</view>
							<view class="u-flex">
								<view class="">
									连载至161章
								</view>
								<view class="">
									更新于今天 21:30
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad(){
			uni.request({
			    url: 'http://121.4.36.25/fiction/search/title/万族之劫//',
			    success: (res) => {
			        console.log(res.data);
			    }
			});
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
