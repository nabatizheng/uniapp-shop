<template>
	<view class="payment-succes">
		<view class="f-active-color">支付成功</view>
		<view @tap="goindex" class="return-index">返回首页</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			goindex(){
				uni.switchTab({
					url:"../index/index"
				})
			}
		}
	}
</script>

<style>
	.payment-succes{
		position: absolute;
		top: 0;
		bottom: 0;
		right: 0;
		left: 0;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.return-index{
		padding: 10rpx 60rpx;
		border: 2px solid #49BDFB;
		border-radius: 40rpx;
		
	}
</style>
