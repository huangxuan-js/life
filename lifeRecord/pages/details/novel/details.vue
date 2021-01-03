<template>
	<view>
		<view class="header">
			<u-navbar back-text="返回"></u-navbar>
			<view class="wrapper">
				<view class="main">
					<view class="u-flex u-col-top u-p-t-30 u-p-b-30">
						<image class="u-p-l-30 u-p-r-20" :src="book.cover" style="width: 150rpx;height: 180rpx;border-radius: 8rpx;" mode="scaleToFill"></image>
						<view class="content">
							<view class="u-bold">
								{{book.title}}
							</view>
							<view class="u-flex u-p-t-10">
								<view class="">
									{{book.author}}
								</view>
								<view class="u-m-l-10">
									〉
								</view>
							</view>
							<view class="u-p-t-10" style="color: #409eff;">
								{{book.fictionType}}
							</view>
						</view>
					</view>
					<view class="container">
						<view class="u-p-l-30 u-p-r-30">
							{{book.descs}}
						</view>
						<navigator :url="'/pages/details/novel/catalog?fictionId='+book.fictionId">
							<view class="u-flex u-row-between u-m-t-30 u-p-l-30 u-p-r-30 u-p-t-30 u-p-b-30 u-border-top u-border-bottom">
								<view class="u-bold u-font-16" style="color: #000000;">
									查看目录
								</view>
								<view class="u-flex u-font-14 u-tips-color">
									<view class="u-m-r-20" >
										更新至{{book.update | text}}
									</view>
									<view class="">
										{{book.updateTime | time}}
									</view>
									<view class="u-m-l-10">
										〉
									</view>
								</view>
							</view>
						</navigator>
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
				book: {
					author: '',
					cover: '',
					descs: '',
					fictionId: '',
					fictionType: '',
					title: '',
					update:'',
					updateTime: ''
				}
			}
		},
		onLoad(data) {
			uni.request({
				url: 'http://121.4.36.25/fictionChapter/search/' + data.fictionId,
				success: (res) => {
					this.book.update = this.$u.trim(res.data.data.data[res.data.data.data.length - 1].title, 'all')
					this.book.author = data.author;
					this.book.cover = data.cover;
					this.book.descs = data.descs;
					this.book.fictionId = data.fictionId;
					this.book.fictionType = data.fictionType;
					this.book.title = data.title;
					this.book.updateTime = data.updateTime;
				}
			});
		},
		filters: {
			text:function(value){
				return value.substr(0, 7) + '...';
			},
			time: function(value) {
				return value.substr(0, 10);
			}
		},
		methods: {

		}
	}
</script>

<style>

</style>
