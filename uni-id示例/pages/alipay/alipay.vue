<template>
	<view class="content">
		<button type="default" @tap="loginByAlipay">支付宝登录</button>
		<button type="default" @tap="code2SessionAlipay">获取支付宝openid</button>
		<button type="default" @tap="bindAlipay">绑定支付宝</button>
		<button type="default" @tap="unbindAlipay">解绑支付宝</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			getAlipayCode() {
				return new Promise((resolve, reject) => {
					uni.login({
						provider: 'alipay',
						success(res) {
							resolve(res.code)
						},
						fail(err) {
							reject(new Error('支付宝登录失败'))
						}
					})
				})
			},
			code2SessionAlipay(){
				this.getAlipayCode().then((code) => {
					return uniCloud.callFunction({
						name: 'user-center',
						data: {
							action: 'code2SessionAlipay',
							params: {
								code,
							}
						}
					})
				}).then((res) => {
					uni.showModal({
						showCancel: false,
						content: JSON.stringify(res.result)
					})
				}).catch((e) => {
					console.error(e)
					uni.showModal({
						showCancel: false,
						content: '获取支付宝openid失败，请稍后再试'
					})
				})
			},
			loginByAlipay() {
				this.getAlipayCode().then((code) => {
					return uniCloud.callFunction({
						name: 'user-center',
						data: {
							action: 'loginByAlipay',
							params: {
								code,
							}
						}
					})
				}).then((res) => {
					uni.showModal({
						showCancel: false,
						content: JSON.stringify(res.result)
					})
					if (res.result.code === 0) {
						uni.setStorageSync('uni_id_token', res.result.token)
						uni.setStorageSync('uni_id_token_expired', res.result.tokenExpired)
					}
				}).catch((e) => {
					console.error(e)
					uni.showModal({
						showCancel: false,
						content: '支付宝登录失败，请稍后再试'
					})
				})
			},
			unbindAlipay() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'unbindAlipay'
					},
					success(res) {
						uni.showModal({
							showCancel: false,
							content: JSON.stringify(res.result)
						})
					},
					fail(e) {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '支付宝解绑失败，请稍后再试'
						})
					}
				})
			},
			bindAlipay() {
				this.getAlipayCode().then((code) => {
					return uniCloud.callFunction({
						name: 'user-center',
						data: {
							action: 'bindAlipay',
							params: {
								code: code,
							}
						}
					})
				}).then((res) => {
					uni.showModal({
						showCancel: false,
						content: JSON.stringify(res.result)
					})
				}).catch((e) => {
					console.error(e)
					uni.showModal({
						showCancel: false,
						content: '支付宝绑定失败，请稍后再试'
					})
				})
			}
		}
	}
</script>

<style>

</style>
