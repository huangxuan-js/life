<template>
	<view>
		<u-search placeholder="搜索" v-model="keyword" @custom='onSearch'></u-search>
		<view class="container u-m-t-40">
			<navigator :url="'/pages/details/novel/details?cover='+item.cover+'&title='+item.title +'&descs='+item.descs+'&author='+item.author+'&fictionType='+item.fictionType+'&fictionId='+item.fictionId+'&updateTime='+item.updateTime"
			 v-for="(item,index) in bookList" :key="index">
				<view class="box u-flex u-col-top u-p-l-20 u-p-r-20 u-p-t-10 u-p-b-10 u-m-b-10 u-white">
					<u-image class="u-m-r-20" style="border-radius: 8rpx;overflow: hidden;" width="120rpx" height="160rpx" :src="item.cover"></u-image>
					<view class="u-relative" style="width: calc(100% - 140rpx);height: 160rpx;">
						<view class="u-m-b-8 u-m-t-4">
							{{item.title}}
						</view>
						<view class="u-line-2 u-font-12 u-tips-color">
							{{item.descs}}
						</view>
						<view class="u-flex u-font-12 u-tips-color u-absolute" style="left: 0;bottom: 0;">
							<view class="u-m-r-8">
								{{item.author}}
							</view>
							<view class="">
								{{item.fictionType}}
							</view>
						</view>
					</view>
				</view>
			</navigator>
			<u-loadmore :status="status" v-show="statusState" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				keyword: '',
				search: '',
				bookList: [],
				from: 1,
				statusState: false,
				status: 'loading',
			};
		},
		onLoad() {

		},
		onReachBottom() {
			this.from = Number(this.from) + 1;
			this.request();
		},
		methods: {
			request() {
				uni.request({
					url: "http://api.pingcc.cn/fiction/search/title/" + this.search + "/" + this.from + "/",
					success: (res) => {
						if (res.data.data.data.length < 10) {
							this.status = 'nomore'
						}
						this.bookList = [...this.bookList, ...res.data.data.data]
					}
				})
			},
			onSearch(e) {
				if (e == '') {
					return;
				}
				if (this.search != this.keyword) {
					this.bookList = []
				}
				this.search = this.keyword
				this.statusState = true
				this.request();
			}

		}
	}
</script>
<style lang="scss">
	page {
		background-color: #f8f8f8;
	}
</style>
<style lang="scss">

</style>
