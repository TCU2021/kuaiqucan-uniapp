<template>
	<view class="background">
		<view class="container">
			<view class="texts">
				<view class="title">请输入柜门号：<input v-model="id"/></view>
				<view class="title">是否发送？</view>
			</view>
			<view class="buttons">
				<u-button class="button" type="primary" size="medium" plain ripple @click="sure()">确认</u-button>
			</view>
			<view class="buttons">
				<u-button class="button" type="primary" size="medium" plain ripple @click="cancel">取消</u-button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad(option) {
			this.id = option.id
		},
		data() {
			return {
				id: '1',
			};
		},
		methods: {
			sure() {
				console.log("确认")
				let that = this
				uni.request({
					url: 'http://81.70.2.91:3000/open/'+that.id,
					// url:'https://www.baidu.com',
					method: 'GET',
					timeout: 3000,
					dataType: 'json',
					success(res) {
						console.log(res.data.code)
						console.log(that.id)
					},
					fail(e) {
						console.log(e)
					}
				})
				uni.redirectTo({
					url: '../result/result'
				});
			},
			cancel() {
				console.log("取消")
				uni.switchTab({
					url: '/pages/index/index'
				});
			}
		},
	}
</script>

<style>
	.container {
		padding-top: 25vh;
	}

	.texts {
		padding-bottom: 5vh;
	}

	.title {
		text-align: center;
	}

	.buttons {
		padding: 1vh 0;
		display: flex;
		align-items: center;
	}

	.button {
		width: 40vw;
		font-size: 30rpx;
	}
</style>
