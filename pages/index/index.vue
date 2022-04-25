<template>
	<view class="bar-bg" v-if="isLoad.window">
		<view class="header">
			<view class="hello">
				<view class="title">欢迎你：{{ user.username }}</view>
				<view class="title">当前待取餐：</view>
			</view>
			<view class="buttons">
				<u-button @click="logout">退出登录</u-button>
			</view>
		</view>
		<view class="todo bg-border">
			<u-loading mode="circle" color="#0184ff" v-if="ordersLoading"></u-loading>
			<view class="content" v-else-if="orders.length>0">
				<swiper class="swiper" @change="changeCurrentPage">
					<swiper-item class="swiperItem" v-for="(item,index) in orders">
						<view class="text">订单名称：{{item.name}}</view>
						<view class="text">所在地：{{item.location}}</view>
						<view class="text">骑手联系电话：{{item.deliverer_telephone}}</view>
						<view class="text">当前状态：{{ stateList[item.state] }}</view>
					</swiper-item>
				</swiper>
				<view class="page">
					{{currentPage+1}}/{{orders.length}}
				</view>
			</view>
			<view class="title" v-else>
				您暂无订单
			</view>
		</view>
		<view class="qrBg" @click="toScan">
			<view class="qrIcon">
				<image src="../../assets/scan_icon.png" class="img" />
				<view class="text">请点击此处扫描二维码</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		stateList
	} from '../../js/state.js'
	import {
		baseUrl
	} from '../../js/base.js'
	export default {
		data() {
			let user = {}
			let orders = [{
				id: 0,
				state: 0
			}]
			let isLoad = {
				window: false
			}
			return {
				currentPage: 0,
				result: '',
				error: '',
				user,
				orders,
				stateList,
				ordersLoading: true,
				isLoad
			}
		},
		methods: {
			toScan() {
				let telephone = this.user.telephone
				let that = this
				uni.scanCode({
					onlyFromCamera: true,
					success: function(res) {
						uni.navigateTo({
							url: '../user/besure/besure?location=' + res.result + '&telephone=' + that
								.user.telephone
						});
						console.log(res.result)
					},
				});
			},
			changeCurrentPage(event) {
				this.currentPage = event.detail.current
			},
			showToast() {
				this.$refs.uToast.show({
					title: '您暂时没有相关订单',
					type: 'warning',
				})
			},
			logout() {
				uni.clearStorageSync()
				uni.reLaunch({
					url: '../Login/Index'
				})
			}
		},
		onShow() {
			this.user = uni.getStorageSync('user')
			if (this.user == '') {
				uni.redirectTo({
					url: '/pages/Login/Index',
					success: (res) => {
						console.log('首页接受数据失败返回登录界面成功', res)
					},
					fail(res) {
						console.log('首页接受数据失败返回登录界面失败', res)
					}
				})
			} else {
				this.isLoad.window = true
				this.ordersLoading = true;
				let that = this
				uni.request({
					url: baseUrl + 'order/getUserOrder',
					method: "POST",
					data: {
						telephone: that.user.telephone
					},
					success: (res) => {
						console.log('订单获取成功', res)
						for (let i = 0; i < res.data.length - 1; i++) {
							for (let j = 0; j < res.data.length - 1 - i; j++) {
								if (res.data[j].state > res.data[j + 1].state) {
									let t = res.data[j + 1]
									res.data[j + 1] = res.data[j]
									res.data[j] = t
								}
							}
						}
						that.orders = res.data
						uni.setStorage({
							key: 'order',
							data: res.data,
							success: function() {
								console.log('订单信息存储成功');
							}
						});
					},
					complete() {
						that.ordersLoading = false;
					}
				})
			}
			// uni.getStorage({
			// 	key: 'orders',
			// 	success(res) {
			// 		that.user.orders = res.data
			// 	},
			// 	fail(res) {
			// 		uni.redirectTo({
			// 			url: '/pages/Login/Index',
			// 			success: (res) => {
			// 				console.log('首页接受数据失败返回登录界面成功', res)
			// 			},
			// 			fail(res) {
			// 				console.log('首页接受数据失败返回登录界面失败', res)
			// 			}
			// 		})
			// 		console.log('fail', res)
			// 	}
			// })
		},
		components: {},
	}
</script>

<style lang="scss">
	.background {
		padding: 40rpx 40rpx 0 40rpx
	}

	.header {
		height: 170rpx;
		display: flex;
		align-items: center;

		.hello {
			height: 170rpx;
			padding: 30rpx 40rpx
		}

		.buttons {
			width: 200rpx;
			margin-left: auto;
			margin-right: 50rpx;
		}
	}

	.todo {
		height: 30%;
		width: 90%;
		display: flex;
		justify-content: center;
		align-items: center;
		margin: 0 auto;

		.content {
			height: 90%;
			width: 90%;
			display: flex;
			flex-direction: column;
			justify-content: center;
			position: relative;

			.swiper {
				height: 100%;
				width: 100%;

				.text {
					display: flex;
					align-items: center;
					height: 25%;

				}
			}

			.page {
				position: absolute;
				// position: relative;
				bottom: 0;
				right: 0;
				font-size: 30rpx;
			}
		}

	}


	.img {
		width: 300rpx;
		height: 300rpx;
	}

	.qrBg {
		height: calc(70% - 170rpx);
		display: flex;
		align-items: center;
		justify-content: center;

		.qrIcon {
			display: flex;
			flex-direction: column;
			align-items: center;
		}
	}
</style>
