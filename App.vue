<script>
	export default {
		onLaunch: function() {
			uni.removeStorageSync('state')
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	@import "uview-ui/index.scss";

	/*每个页面公共css */
	* {
		padding: 0;
		margin: 0;
	}

	body {
		// background-image: url('./assets/background.jpg');
		background-color: rgba($color: #f2f2f2, $alpha: 1.0);
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}

	.back {
		display: flex;
		align-items: center;
	}

	.container {
		padding-top: 60rpx;
		width: 100%;
		height: calc(100% - 60rpx);
		flex-direction: column;
		display: flex;
		align-items: center;
	}

	.title {
		font-size: 45rpx;
		font-weight: 600;
	}

	.big-text {
		font-size: 40rpx;
	}

	.weight-text {
		font-size: 35rpx;
		font-weight: 600;
	}

	.text {
		font-size: 35rpx;
	}

	.little-text {
		font-size: 30rpx;
	}

	.img {
		height: 60rpx;
		width: 60rpx;
	}

	.bg-border {
		background-color: rgba(255, 255, 255, 0.7);
		border-radius: 30rpx;
	}

	.back {
		width: 100%;
		height: 100rpx;
		display: flex;
		align-items: center;
	}

	/*每个页面公共css */
	.bar-bg {
		height: calc(100vh - 100px);
		width: 100vw;
	}

	.bg {
		height: 100vh;
		width: 100vw;
	}
	
	.background{
		height: 100vh;
		width: 100vw;
	}
</style>
