<template>
	<view class="content">
		<input type="text" placeholder="邮箱" v-model="email" />
		<view class="tips" style="color: red;">上次发送的验证码：{{code}}</view>
		<button type="default" @tap="setVerifyCode('login')">设置验证码（注册&登录）</button>
		<button type="default" @tap="loginByEmail">邮箱验证码登录（不存在则注册）</button>
		<button type="default" @tap="setVerifyCode('bind')">设置验证码（绑定邮箱）</button>
		<button type="default" @tap="bindEmail">绑定邮箱</button>
		<view class="tips">绑定邮箱时如果不传验证码或者传空则直接绑定并设置email_confirmed为1，不会去校验验证码</view>
		<button type="default" @tap="setVerifyCode('unbind')">设置验证码（解绑邮箱）</button>
		<button type="default" @tap="unbindEmail">解绑邮箱</button>
		<view class="tips">解绑邮箱时如果不传验证码或者传空则直接解除绑定绑定，不会去校验验证码</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				email: '',
				code: ''
			}
		},
		methods: {
			// 为了演示把这个逻辑放在客户端
			getCode() {
				const randomStr = '00000' + Math.floor(Math.random() * 1000000)
				this.code = randomStr.substring(randomStr.length - 6)
			},
			setVerifyCode(type) {
				if (!/.+@.+/.test(this.email)) {
					uni.showModal({
						content: '请输入正确的邮箱',
						showCancel: false
					})
					return
				}
				this.getCode()
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'setVerifyCode',
						params: {
							email: this.email,
							code: this.code,
							type
						}
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
							content: '发送失败，请稍后再试'
						})
					}
				})
			},
			loginByEmail() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'loginByEmail',
						params: {
							email: this.email,
							code: this.code
						}
					},
					success(res) {
						uni.showModal({
							showCancel: false,
							content: JSON.stringify(res.result)
						})
						if (res.result.code === 0) {
							uni.setStorageSync('uni_id_token', res.result.token)
							uni.setStorageSync('uni_id_token_expired', res.result.tokenExpired)
						}
					},
					fail(e) {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '登录失败，请稍后再试'
						})
					}
				})
			},
			bindEmail() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'bindEmail',
						params: {
							email: this.email,
							code: this.code
						}
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
							content: '绑定失败，请稍后再试'
						})
					}
				})
			},
			unbindEmail() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'unbindEmail',
						params: {
							email: this.email,
							code: this.code
						}
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
							content: '解绑失败，请稍后再试'
						})
					}
				})
			}
		}
	}
</script>

<style>

</style>
