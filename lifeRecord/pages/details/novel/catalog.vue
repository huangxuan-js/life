<template>
	<view>
		<u-navbar ></u-navbar>
		<view class="container">
			<navigator open-type="redirect" :url="'/pages/details/novel/read?chapterId=' + index" v-for="(item,index) in catalogList" :key="index">
				<view class="u-p-l-20 u-p-t-30 u-p-b-30 u-border-bottom" :class="{'active' : index == active}"  >
					{{item.title}}
				</view>
			</navigator>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				catalogList:[],
				active:0,
			}
		},
		onLoad(data){
			uni.request({
			    url: 'http://121.4.36.25/fictionChapter/search/'+data.fictionId,
			    success: (res) => {
					console.log(res)
					this.catalogList = res.data.data.data
					uni.setStorageSync('catalogList', res.data.data.data);
			    }
			});
		},
		methods: {
			
		}
	}
</script>

<style>
	.active{
		color: #24acf2;
	}
</style>
