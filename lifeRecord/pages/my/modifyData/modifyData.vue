<template>
	<view>
		<view class="u-m-t-80 u-m-b-40 u-p-t-40 u-flex u-row-center">
			<u-avatar :src="pic" size="140"></u-avatar>
		</view>
		<u-cell-group>
				<u-cell-item :title-style="{'color':'#000000','font-size':'32rpx'}"  title="用户名"   value="黄旋"></u-cell-item>
				<u-cell-item :title-style="{'color':'#000000','font-size':'32rpx'}" title="会员等级" value="男"></u-cell-item>
				<u-cell-item :title-style="{'color':'#000000','font-size':'32rpx'}" title="个性签名" value="张三李四王二麻子黄建彬啊..."></u-cell-item>
				<u-cell-item :title-style="{'color':'#000000','font-size':'32rpx'}" title="装扮礼物" value=""></u-cell-item>
		</u-cell-group>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pic:''
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
