<template>
	<view class="content">
		<button type="default" @tap="setAvatar">设置头像</button>
		<button type="default" @tap="updateUser">更新用户信息</button>
		<button type="default" @tap="checkToken">token校验</button>
		<button type="default" @tap="encryptPwd">密码加密测试</button>
		<button type="default" @tap="setUserInviteCode">设置邀请码</button>
		<div class="tips">设置邀请码时如果传入邀请码则设置传入的邀请码，如果不传则随机生成6位邀请码</div>
		<input type="text" v-model="inviteCode" placeholder="邀请码" />
		<button type="default" @tap="acceptInvite">接受邀请</button>
		<button type="default" @tap="logout">退出</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inviteCode: ''
			}
		},
		methods: {
			setAvatar() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'setAvatar',
						params: {
							avatar: 'https://img.cdn.aliyun.dcloud.net.cn/uni-app/uniCloud/unicloudlogo.png'
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
							content: '设置失败，请稍后再试'
						})
					}
				})
			},
			updateUser() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'updateUser',
						params: {
							nickname: '法外狂徒张三'
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
							content: '请求云函数失败，请稍后再试'
						})
					}
				})
			},
			checkToken() {
				console.warn('注意1.1.0版本会返回userInfo，请不要返回全部信息给客户端')
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'checkToken'
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
							content: '请求云函数失败，请稍后再试'
						})
					}
				})
			},
			encryptPwd() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'encryptPwd'
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
							content: '重置失败，请稍后再试'
						})
					}
				})
			},
			logout() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'logout'
					},
					success(res) {
						uni.showModal({
							showCancel: false,
							content: JSON.stringify(res.result)
						})
						uni.removeStorageSync('uni_id_token')
						uni.removeStorageSync('uni_id_token_expired')
					},
					fail(e) {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '登出失败，请稍后再试'
						})
					}
				})
			},
			setUserInviteCode() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'setUserInviteCode'
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
							content: '设置失败，请稍后再试'
						})
					}
				})
			},
			acceptInvite() {
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
						action: 'acceptInvite',
						params: {
							inviteCode: this.inviteCode
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
							content: '接受邀请失败，请稍后再试'
						})
					}
				})
			}
		}
	}
</script>

<style>

</style>
