<template>
	<view class='bg'>
		<u-toast ref="uToast" />
		<view class="logo">
			<image src="../../assets/logo.png" mode="aspectFit" class="logo"></image>
		</view>
		<view class="username item">
			<image src="../../static/user.svg" class="img"></image>
			<input placeholder="请输入账号" class="input" v-model="username" />
		</view>
		<view class="password item">
			<image src="../../static/lock.svg" class="img"></image>
			<input class="input" placeholder="请输入密码" v-model="password" password />
		</view>
		<view class="password item">
			<image src="../../static/phone.svg" class="img"></image>
			<input class="input" placeholder="请输入联系电话" v-model="telephone" />
		</view>
		<view class="item">
			<image src="../../static/location.svg" class="img"></image>
			<input class="input" placeholder="请输入配送地址" v-model="location" />
		</view>
		<u-button class="button" @click="register">注册</u-button>
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
				password: '',
				telephone: '',
				location: ''
			}
		},
		methods: {
			register() {
				let that = this
				uni.request({
					url: baseUrl + 'user/register',
					method: 'POST',
					data: {
						user: {
							username: that.username,
							password: that.password,
							telephone: that.telephone,
							location: that.location,
						}
					},
					success(res) {
						console.log(res)
						if (res.data.state) {
							uni.setStorageSync('user', res.data.data);
							uni.reLaunch({
								url: '/pages/index/index',
								success(res) {
									console.log('注册转跳首页成功', res)
								},
								fail(e) {
									console.log('注册转跳首页失败', e)
								}
							})
						} else {
							that.$refs.uToast.show({
								title: '注册失败\n' + res.data.message,
								type: 'warning',
								position: 'top',
							})
						}
					},
					fail(res) {
						console.log(res)
					}
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.bg {
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
			margin-top: 20rpx;
		}
	}
</style>
