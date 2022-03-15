<template>
	<view class="bg">
		<u-toast ref="uToast" />
		<view class="container">
			<view class="texts" v-if="have">
				<view class="title">您的外卖当前所在柜门号为：</view>
				<view class="title" style="font-size: 30px" v-if="!loading">{{order.number}}</view>
				<u-loading mode="circle" color="#0184ff" v-else></u-loading>
				<view class="title">是否开箱？</view>
			</view>
			<view class="texts" v-else>
				<view class="title">
					您暂时没有订单
				</view>
			</view>
			<view class="buttons">
				<u-button class="button" type="primary" size="medium" plain ripple @click="sure" v-if="have">确认
				</u-button>
				<u-button class="button" type="primary" size="medium" plain ripple @click="cancel">取消</u-button>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		baseUrl
	} from '../../../js/base.js'
	export default {
		onLoad(option) {
			this.loading = true
			let that = this
			uni.request({
				url: baseUrl + 'order/getCabinetOrder',
				method: 'POST',
				data: {
					telephone: option.telephone,
					location: option.location,
				},
				success: (res) => {
					console.log('开柜订单请求成功', res.data)
					if (res.data.state) {
						that.order = res.data.data
					} else {
						that.have = false
					}
					that.loading = false
				},
				fail: (e) => {
					console.log('开柜订单请求失败', e)
				}
			})
		},
		data() {
			return {
				loading: true,
				have: true,
				order: {}
			};
		},
		methods: {
			sure() {
				let that = this
				uni.request({
					// url: 'http://81.70.2.91:3000/open/01',
					url: baseUrl + 'order/finish',
					method: 'POST',
					data: {
						order: that.order
					},
					success(res) {
						console.log(res.data)
						if (res.data.state) {
							uni.redirectTo({
								url: '/pages/user/result/result'
							});
						} else {
							that.showToast()
						}
					},
					fail(e) {
						console.log(e)
					}
				})

			},
			cancel() {
				console.log("取消")
				uni.switchTab({
					url: '/pages/index/index'
				});
			},
			showToast() {
				this.$refs.uToast.show({
					title: '打开失败,请重试',
					type: 'warning',
				})
			}
		},
		onShow() {

		}
	}
</script>

<style>
	.background {
		height: 100%;
		width: 100%;
	}

	.container {
		height: 100%;
		width: 100%;
	}

	.texts {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding-bottom: 5vh;
		height: 40%;
	}

	.title {
		height: 15%;
		text-align: center;
	}

	.buttons {
		height: 60%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.button {
		width: 40vw;
		margin: 30rpx 0;
		font-size: 30rpx;
	}
</style>
