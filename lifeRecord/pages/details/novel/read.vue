<template>
	<view>
		<view class="container u-font-16 u-p-l-30 u-p-r-30">
			<view class="u-m-t-20 u-m-b-20" v-for="(item,index) in textList" :key="index">
				{{item | textReplace }}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				count: 0,
				textList: [],
				catalogList: [],
			}
		},
		onLoad(data) {
			this.count = data.chapterId;
			try {
				this.catalogList = uni.getStorageSync('catalogList');
				if (this.catalogList) {
					this.request(this.catalogList[data.chapterId].chapterId)
				}
			} catch (err) {
				console.log(err)
			}
		},
		filters: {
			textReplace: function(value) {
				return value.replace(/&nbsp;/g, "");
			}
		},
		onReachBottom() {
			this.count = Number(this.count) + 1;
			this.request(this.catalogList[this.count].chapterId)
		},
		methods: {
			request(data) {
				uni.request({
					url: 'http://api.pingcc.cn/fictionContent/search/' + data,
					success: (res) => {
						this.textList = [...res.data.data.data[0].content, ...this.textList]
					}
				});
			}
		}
	}
</script>

<style>

</style>
