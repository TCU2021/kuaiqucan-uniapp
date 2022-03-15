<template>
	<view class="background">
		<view class="text">
			柜门已打开
		</view>
		<view class="buttons">
			<u-button class="button" type="primary" size="medium" plain ripple @click="back">返回</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {
			back() {
				uni.switchTab({
					url: '/pages/index/index',
				});
			}
		}
	}
</script>

<style>
	.text {
		padding-top: 30vh;
		font-size: 100rpx;
		text-align: center;
	}

	.buttons {
		padding-top: 5vh;
		display: flex;
		align-items: center;
	}
</style>
