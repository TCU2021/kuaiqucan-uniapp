<template>
	<view class='background'>
		<u-toast ref="uToast" />
		<view class="logo">
			<image src="../../assets/logo.png" mode="aspectFit" class="logo"></image>
		</view>
		<view class="username item">
			<image src="../../static/user.svg" class="img"></image><input placeholder="请输入账号" class="input"
				v-model="username" />
		</view>
		<view class="password item">
			<image src="../../static/lock.svg" class="img"></image><input class="input" placeholder="请输入密码"
				v-model="password" password />
		</view>
		<u-button class="button" @click="login">登录</u-button>
	</view>
</template>

<script>
	import {
		baseUrl
	} from '../../js/base.js'
	export default {
		data() {
			return {
				username: '',
				password: ''
			}
		},
		methods: {
			login() {
				let that = this
				uni.request({
					url: baseUrl + 'user/login',
					method: "POST",
					data: {
						username: that.username,
						password: that.password
					},
					success: (res) => {
						console.log('登录请求访问成功', res.data);
						if (res.data.state) {
							uni.setStorageSync('user', res.data.data);
							uni.switchTab({
								url: '/pages/index/index',
								success(res) {
									console.log('登录转跳首页成功', res)
								},
								fail(e) {
									console.log('登录转跳首页失败', e)
								}
							})
						} else {
							that.$refs.uToast.show({
								title: '登录失败',
								type: 'warning',
								position: 'top',
							})
						}
					},
					fail: (err) => {
						console.error('请求失败', err)
						that.$refs.uToast.show({
							title: '登录失败',
							type: 'warning',
							position: 'top',
						})
					}
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	.background {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;

		.logo {
			height: 200rpx;
			width: 200rpx;
		}

		.item {
			height: 100rpx;
			width: 90%;
			margin: 20rpx auto;
			display: flex;
			align-items: center;
			background-color: rgba($color: #ffffff, $alpha: 1.0);

			.img {
				width: 20%;
				// height: 100%;
			}

			.input {
				width: 80%;
				height: 100%;
			}
		}

		.button {
			width: 90%;
		}
	}
</style>
