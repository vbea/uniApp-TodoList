<template>
	<view class="container">
		<input class="txt-input" type="text" v-model="title" placeholder="Title" maxlength="30"/>
		<textarea class="txt-content" auto-height placeholder="Content(optional)" v-model="content" maxlength="500"></textarea>
		<view class="btnAdd">
			<button class="ui-button" hover-class="button-hover" @click="addData">Add</button>
		</view>
		
	</view>
</template>

<script>
	var Bmob;
	export default {
		data() {
			return {
				title: "",
				content: ""
			}
		},
		onLoad:function(){
			Bmob = getApp().Bmob;
		},
		methods: {
			addData: function(e) {
				if (!this.title.trim()) {
					uni.showToast({
						title:"请输入标题",
						icon:"none"
					})
					return
				}
				uni.showLoading({
					title:"请稍后..."
				})
				const query = Bmob.Query('list');
				query.set("title", this.title)
				query.set("content", this.content)
				query.save().then(res => {
				  console.log("添加数据", res)
				  uni.showToast({
				  	title:"添加成功"
				  })
				  setTimeout(function() {
					  uni.navigateBack()
				  }, 500)
				}).catch(err => {
				  console.error("添加数据", err)
				  uni.showToast({
				  	title:"添加失败，请重试",
				  	icon:"none"
				  })
				})
			}
		}
	}
</script>

<style>
	page {
		overflow: hidden;
	}
	
	.txt-content {
		width: auto;
		display: flex;
		padding: 30rpx;
		font-size: 30rpx;
		min-height: 100rpx;
	}
	
	.txt-input {
		padding: 10rpx;
		font-size: 30rpx;
		transition: all .3s;
		margin: 20rpx 20rpx 0;
		border-bottom: 1px solid #eee;
	}
	
	.txt-input:active,
	.txt-input:focus-within {
		box-shadow: 0 1px 0 0 #88B04B;
		border-bottom: 1px solid #88B04B !important;
	}
	
	.btnAdd {
		left: 0;
		right: 0;
		z-index: 5;
		bottom: 30rpx;
		position: fixed;
	}
</style>
