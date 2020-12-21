<template>
	<view>
		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-30 u-p-t-30" style="background-color: #ffffff;">
			<view class="u-m-r-10">
				<navigator url="/pages/my/modifyData/modifyData" >
					<u-avatar :src="pic" size="140"></u-avatar>
				</navigator>
			</view>
			<view class="u-flex-1">
				<view class="u-font-18 u-p-b-20">黄旋</view>
				<view class="u-flex u-font-14 u-tips-color">
					<view class="u-line-1" style="width: 360rpx;">
						张三李四王二麻子黄建彬啊啊啊啊
					</view>
					<u-icon class="u-margin-left-10" name="edit-pen" color="#000000"></u-icon>
				</view>
			</view>
			<view class="u-m-l-10 u-p-10">
				<navigator url="/pages/my/modifyData/modifyData" >
					<u-icon name="arrow-right" color="#969799" size="28"></u-icon>
				</navigator>
			</view>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="chat" title="消息通知"></u-cell-item>
			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="clock" title="最近浏览"></u-cell-item>
				<u-cell-item icon="star" title="我的收藏"></u-cell-item>
				<u-cell-item icon="share" title="分享应用"></u-cell-item>
				<u-cell-item icon="download" title="缓存"></u-cell-item>
			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="setting" title="设置"></u-cell-item>
			</u-cell-group>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pic: "https://thirdwx.qlogo.cn/mmopen/vi_32/M8CYVNHVBpSH9uUFfibjc3yYaczJMbcncKJnhRF7vs7paQcjePajj1qJaIkiauqGqunNIjevnk65iaticNkZtl4Sog/132",
			}
		},
		onLoad() {
		},
		methods: {

		}
	}
</script>

<style lang="scss">
	page {
		background-color: #ededed;
	}
</style>
