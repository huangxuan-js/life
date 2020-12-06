<template>
	<view class="content">
		<view class="tips">考虑到性能，登录操作时传入参数needPermission: true时，checkToken时才会返回Permission，否则只会返回role。Permission一般用于管理后台</view>
		<button type="default" @tap="addPermission">新增权限</button>
		<button type="default" @tap="addRole">新增角色</button>
		<view class="role-permission">
			<scroll-view class="role-scroll" scroll-y="true">
				<view>角色列表</view>
				<checkbox-group @change="roleChange">
					<view v-for="(item,index) in roleList" :class="{act: currentRoleIndex === index}" @click="selectRole(index)" class="role-item">
						<checkbox :value="item.role_id" :checked="userRole.indexOf(item.role_id) > -1" /><text>{{item.role_name}}</text>
					</view>
				</checkbox-group>
			</scroll-view>
			<scroll-view class="permission-scroll" scroll-y="true">
				<view>权限列表</view>
				<checkbox-group @change="permissionChange">
					<view v-for="(item,index) in permissionList" class="permission-item">
						<checkbox :value="item.permission_id" :checked="currentRolePermission.indexOf(item.permission_id) > -1" /><text>{{item.permission_name}}</text>
					</view>
				</checkbox-group>
			</scroll-view>
		</view>
		<button type="default" @tap="bindRole">为用户绑定角色</button>
		<button type="default" @tap="bindPermission">为角色绑定权限</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				roleList: [],
				permissionList: [],
				userRole: [],
				currentRoleIndex: 0,
				currentRolePermission: []
			}
		},
		onLoad() {
			this.getRoleList()
			this.getPermissionList()
			this.getRoleByUid()
		},
		methods: {
			selectRole(index) {
				this.currentRoleIndex = index
				const currentRole = this.roleList[this.currentRoleIndex]
				if (!currentRole) {
					this.currentRolePermission = []
				}
				this.currentRolePermission = currentRole.permission
			},
			roleChange(e) {
				this.userRole = e.detail.value
			},
			permissionChange(e) {
				this.currentRolePermission = e.detail.value
			},
			addRole() {
				const roleTag = Math.random().toString(32).substr(2)
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'addRole',
						params: {
							roleID: 'r_id_' + roleTag,
							roleName: 'r_name_' + roleTag,
							comment: 'comment' + roleTag,
							permission: []
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						uni.showModal({
							content: '角色新增成功',
							showCancel: false
						})
						this.getRoleList()
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			addPermission() {
				const permissionTag = Math.random().toString(32).substr(2)
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'addPermission',
						params: {
							permissionID: 'p_id_' + permissionTag,
							permissionName: 'p_name_' + permissionTag,
							comment: 'comment' + permissionTag
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						uni.showModal({
							content: '权限新增成功',
							showCancel: false
						})
						this.getPermissionList()
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			bindRole() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'bindRole',
						params: {
							roleList: this.userRole,
							// 设置reset为true，整体覆盖。设置为false时增量更新role
							reset: true,
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						uni.showModal({
							content: '角色绑定成功',
							showCancel: false
						})
						this.getRoleByUid()
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			bindPermission() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'bindPermission',
						params: {
							roleID: this.roleList[this.currentRoleIndex].role_id,
							permissionList: this.currentRolePermission,
							// 设置reset为true，整体覆盖。设置为false时增量更新role
							reset: true,
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						uni.showModal({
							content: '权限绑定成功',
							showCancel: false
						})
						this.getRoleList()
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			getRoleList() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'getRoleList',
						params: {
							offset: 0,
							limit: 10000,
							needTotal: false
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						this.roleList = res.result.roleList
						this.selectRole(this.currentRoleIndex)
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			getPermissionList() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'getPermissionList',
						params: {
							offset: 0,
							limit: 10000,
							needTotal: false
						}
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						this.permissionList = res.result.permissionList
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			},
			getRoleByUid() {
				uniCloud.callFunction({
					name: 'user-center',
					data: {
						action: 'getRoleByUid'
					},
					success: (res) => {
						if (res.result.code !== 0) {
							uni.showModal({
								showCancel: false,
								content: res.result.msg
							})
							return
						}
						this.userRole = res.result.role
					},
					fail: (e) => {
						console.error(e)
						uni.showModal({
							showCancel: false,
							content: '请求服务失败，请稍后再试'
						})
					}
				})
			}
		}
	}
</script>

<style>
	.role-permission {
		display: flex;
		height: 400px;
		margin-bottom: 15px;
	}

	.role-scroll,
	.permission-scroll {
		flex: 1;
	}

	.role-item,
	.permission-item {
		padding: 5px;
	}

	.role-item text,
	.permission-item text {
		display: block;
		font-size: 14px;
	}

	.role-scroll .role-item {
		position: relative;
	}
	
	.role-scroll .role-item.act::after {
		position: absolute;
		content: "→";
		color: #007AFF;
		font-size: 20px;
		height: 20px;
		line-height: 20px;
		right: 5px;
		top: 50%;
		margin-top: -10px;
	}
</style>
