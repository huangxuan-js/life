<template>
	<view>
		<view class="u-relative" style="height: 400rpx;">
			<view class="top-wpr u-relative">
				<view class="u-flex u-row-right u-p-t-40 u-p-r-30">
					<u-icon name="search" color="#000000" size="50" class="u-m-r-40"></u-icon>
					<u-icon name="more-dot-fill" color="#000000" size="50" style="transform: rotate(90deg);"></u-icon>
				</view>
				<view class="u-flex u-row-between u-p-l-30 u-p-r-30" style="color: #ffffff;">
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
						<u-notice-bar mode="vertical" :volume-icon="false" :more-icon="true" type="none" :list="recommendList"></u-notice-bar>
					</view>
				</view>
			</view>
		</view>
		<view class="u-m-t-20 u-p-l-30 u-p-r-30 ">
			<view class="book-item u-relative u-flex u-m-b-30" v-for="(item,index) in bookList" :key="index" @longtap="longtap(index)">
				<u-image width="106rpx" height="130rpx" :src="item.avatar" class="u-m-r-20" style="border-radius: 8rpx;overflow: hidden;"></u-image>
				<view class="">
					<view class="u-m-b-20">
						{{item.name}}
					</view>
					<view class="book-describe ">
						{{item.describe}}
					</view>
				</view>
				<button class="u-reset-button u-font-12 icon-update" v-show="false">更新</button>
				<view class="u-flex-1 u-flex u-row-right">
					<u-checkbox-group >
						<u-checkbox v-model="item.check" v-show="checked" shape="circle" @change="checkChange"></u-checkbox>
					</u-checkbox-group>
				</view>
			</view>
			<view class="book-item u-relative u-flex u-m-b-30" style="font-size: 0;" v-show="!checked">
				<view class="u-m-r-20  book-recommend">
					+
				</view>
				<view class="book-describe u-font-14">
					更多精彩小说
				</view>
			</view>
			
		</view>
		<view class="u-fixed u-flex" style="left: 0;top: 0;height: 100rpx;background-color: #ffffff;width: 100%;" v-show="checked">
			<view class="u-flex-1 u-p-l-40" @click="checkAll">
				{{title}}
			</view>
			<view class="u-flex-1 u-text-center u-font-18" style="color:#000000">
				书架
			</view>
			<view class="u-flex-1 u-text-right u-p-r-40" @click="complete">
				完成
			</view>
		</view>
		<view class="book-select" v-show="checked">
			<view class="u-flex u-row-between u-p-t-30 u-p-b-20 u-p-l-40 u-p-r-40" style="box-shadow: 0 -2px 4px -1px hsla(0,6%,58%,.6);border-radius: 6rpx;">
				<view class="">
					<view class="u-bolder">
						{{bookDescribe.name}}
					</view>
					<view class="u-font-12 u-m-t-10"style="color: #2979ff;">
						查看详情>
					</view>
				</view>
				<view class="">
					<u-icon name="share" size="50"></u-icon>
				</view>
			</view>
			<view class="u-flex u-row-between u-text-center u-border-top" style="height: 100rpx; ">
				<view class="u-flex-1" >
					置顶
				</view>
				<view class="u-flex-1" style="color: #fa3534;">
					删除(1)
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				recommendList: [
					'寒雨连江夜入吴',
					'平明送客楚山孤',
					'洛阳亲友如相问',
					'一片冰心在玉壶'
				],
				bookList:[
					{
						avatar:"https://ggzqimg.cdn.bcebos.com/BookFiles/BookImages/zhangyeyuhuo.jpg",
						name:"长夜余火",
						describe:"更新至第569话  正确的选择",
						check:false,
						update:true,
					},
					{
						avatar:"https://www.miaojiang8.com/files/article/image/6/6233/6233s.jpg",
						name:"突然成了仙怎么办",
						describe:"更新至第569话  不正确的选择",
						check:false,
						update:true,
					},
				],
				bookDescribe:{},
				title:"全选",
				checked: false,
				iconBook: require('../../static/icons/book.png'),
			}
		},
		onLoad() {
		},
		methods: {
			// 长按显示编辑
			longtap(i){
				if(!this.checked){
					this.bookList[i].check = true
					this.checked = true
					this.checkChange();
					uni.hideTabBar({})
					this.bookDescribe =this.$u.deepClone(this.bookList[i]) 	
				}
			},
			// 全选 取消全选
			checkAll(){
				if(this.title == '全选'){
					this.bookList.forEach(v=>{
						v.check = true
					})
					this.title = '取消全选'
				}else{
					this.bookList.forEach(v=>{
						v.check = false
					})
					this.title = '全选'
				}
			},
			// book选中状态
			checkChange(e){
				
			},
			// 完成
			complete(){
				this.checked = false
				uni.showTabBar({})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.top-wpr {
		width: 100%;
		height: 360rpx;
		background: linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), url(https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-wwgjizeoojbef6e56c/24d77650-41d2-11eb-b997-9918a5dda011.jpg);
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
	.book-recommend{
		width: 106rpx;
		height: 130rpx;
		border-radius: 8rpx;
		overflow: hidden;
		border: 1rpx solid #c0c4cc;
		text-align: center;
		color: $u-type-info;
		font-size: 60rpx;
		line-height: 114rpx;
	}
	.book-select{
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100%;
		background-color: #ffffff;
	}
</style>
