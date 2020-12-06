<template>
	<view class="content">
		<input type="text" placeholder="手机号" v-model="mobile" />
		<view class="tips" style="color: red;">上次发送的验证码：{{code}}</view>
		<button type="default" @tap="sendSmsCode('login')">发送验证码（不存在则注册）</button>
		<button type="default" @tap="setVerifyCode('login')">设置验证码（不存在则注册）</button>
		<button type="default" @tap="loginBySms">短信验证码登录（不存在则注册）</button>
		<button type="default" @tap="loginOnly">短信验证码登录（只允许登录）</button>
		<button type="default" @tap="sendSmsCode('register')">发送验证码（仅注册）</button>
		<button type="default" @tap="setVerifyCode('register')">设置验证码（仅注册）</button>
		<input type="text" placeholder="邀请码" v-model="inviteCode" />
		<button type="default" @tap="loginBySmsInvite">短信验证码注册（包含邀请码）</button>
		<view class="tips">如果不使用uni-id自带的发送验证码功能，可以自行调用接口设置验证码，参数与发送验证码一致。loginBySms时可以传入type参数指明只允许登录（login）或只允许注册（register）。针对不传type的情况（即存在手机号则登录，不存在则注册）请在发送短信验证码是设置类型为login，如果传了type需要在发送短信验证码时设置为type同样的类型</view>
		<button type="default" @tap="sendSmsCode('bind')">发送验证码（绑定手机）</button>
		<button type="default" @tap="setVerifyCode('bind')">设置验证码（绑定手机）</button>
		<button type="default" @tap="bindMobile">绑定手机号</button>
		<view class="tips">绑定手机时如果不传验证码或者传空则直接绑定并设置mobile_confirmed为1，不会去校验验证码</view>
		<button type="default" @tap="sendSmsCode('unbind')">发送验证码（解绑手机）</button>
		<button type="default" @tap="setVerifyCode('unbind')">设置验证码（解绑手机）</button>
		<button type="default" @tap="unbindMobile">解绑手机</button>
		<view class="tips">解绑手机时如果不传验证码或者传空则直接解除绑定绑定，不会去校验验证码</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mobile: '',
				code: '',
				inviteCode: ''
			}
		},
		methods: {
			// 为了演示把这个逻辑放在客户端
			getCode() {
				const randomStr = '00000' + Math.floor(Math.random() * 1000000)
				this.code = randomStr.substring(randomStr.length - 6)
			},
			sendSmsCode(type) {
				if (!/^1\d{10}$/.test(this.mobile)) {
					uni.showModal({
						content: '请输入正确的手机号',
						showCancel: false
					})
					return
				}
				this.getCode()
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'sendSmsCode',
						params: {
							mobile: this.mobile,
							code: this.code,
							type,
							templateId: '10109'
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
			setVerifyCode(type) {
				if (!/^1\d{10}$/.test(this.mobile)) {
					uni.showModal({
						content: '请输入正确的手机号',
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
							mobile: this.mobile,
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
			loginBySms() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'loginBySms',
						params: {
							mobile: this.mobile,
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
			loginOnly() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'loginBySms',
						params: {
							type: 'login',
							mobile: this.mobile,
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
			loginBySmsInvite() {
				if (!this.inviteCode) {
					uni.showModal({
						content: '请填写邀请码',
						showCancel: false
					})
					return
				}
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'loginBySms',
						params: {
							type: 'register',
							mobile: this.mobile,
							code: this.code,
							inviteCode: this.inviteCode
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
			bindMobile() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'bindMobile',
						params: {
							mobile: this.mobile,
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
			unbindMobile() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'unbindMobile',
						params: {
							mobile: this.mobile,
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
			},
		}
	}
</script>

<style>

</style>
