<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view @click="toAdd">Hello world</view>
	</view>
</template>

<script>
	import Bmob from "hydrogen-js-sdk";
	import moment from "moment";
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
			//Bmob.initialize("5c718880a0a2aee46502557392e705fa", "0364c7");
			//Bmob.debug(true)
			let a = 100;
			console.log("onLoad", moment("").format("YYYY-MM-DD HH:mm"));
		},
		methods: {
			toAdd: function(e) {
				console.log(e);
				uni.navigateTo({
					url:"../Add/Add"
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
