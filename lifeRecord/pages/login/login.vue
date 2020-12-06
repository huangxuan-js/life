<template>
	<view class="content">
		<!-- #ifdef MP-WEIXIN -->
		<button type="default" @tap="loginByWeixin">微信登录</button>
		<!-- #endif -->
	</view>
</template>

<script>
	let weixinAuthService
	export default {
		data() {
			return {
				hasWeixinAuth: false
			}
		},
		onLoad() {
			// #ifdef APP-PLUS
			plus.oauth.getServices((services) => {
				weixinAuthService = services.find((service) => {
					return service.id === 'weixin'
				})
				if (weixinAuthService) {
					this.hasWeixinAuth = true
				}
			});
			// #endif	
		},
		methods: {
			getWeixinCode() {
				return new Promise((resolve, reject) => {
					// #ifdef APP-PLUS
					weixinAuthService.authorize(function(res) {
						resolve(res.code)
					}, function(err) {
						console.log(err)
						reject(new Error('微信登录失败'))
					});
					// #endif
					// #ifdef MP-WEIXIN
					uni.login({
						provider: 'weixin',
						success(res) {
							resolve(res.code)
						},
						fail(err) {
							reject(new Error('微信登录失败'))
						}
					})
					// #endif
				})
			},
			loginByWeixin() {
				this.getWeixinCode().then((code) => {
					return uniCloud.callFunction({
						name: 'user-center',
						data: {
							action: 'loginByWeixin',
							params: {
								code,
							}
						}
					})
				}).then((res) => {
					if (res.result.code === 0) {
						uni.setStorageSync('uni_id_token', res.result.token)
						uni.setStorageSync('uni_id_token_expired', res.result.tokenExpired)
						uni.getUserInfo({
							provider: 'weixin',
							success: function(res) {
								console.log(res)
							}
						});
					}
				}).catch((e) => {
					console.error(e)
					uni.showModal({
						showCancel: false,
						content: '微信登录失败，请稍后再试'
					})
				})
			},
		}
	}
</script>

<style>

</style>
