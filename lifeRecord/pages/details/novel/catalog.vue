<template>
	<view>
		<u-navbar ></u-navbar>
		<view class="container">
			<view class="u-p-l-20 u-p-t-30 u-p-b-30 u-border-bottom" :class="{'active' : index == active}" v-for="(item,index) in catalogList" :key="index">
				{{item.title}}
			</view>
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
		onLoad(){
			uni.request({
			    url: 'http://api.pingcc.cn/fictionChapter/search/127557',
			    success: (res) => {
					this.catalogList = res.data.data.data
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
