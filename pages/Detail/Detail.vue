<template>
	<view class="container">
		<input class="txt-input" type="text" v-model="todo.title" placeholder="Title" maxlength="30"/>
		<textarea class="txt-content" auto-height placeholder="Content(optional)" v-model="todo.content" maxlength="500"></textarea>
		<view class="btnAdd">
			<button class="ui-button" hover-class="button-hover" @click="saveData">Save</button>
		</view>
	</view>
</template>

<script>
	var Bmob;
	export default {
		data() {
			return {
				todo: {
					title: "",
					content: ""
				}
			}
		},
		onLoad:function(){
			Bmob = getApp().Bmob;
			if (getApp().globalData.itemData) {
				this.todo = getApp().globalData.itemData
			} else {
				uni.reLaunch({
					url:"../index/index"
				})
			}
		},
		methods: {
			saveData: function(e) {
				if (!this.todo.title.trim()) {
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
				query.set("id", this.todo.objectId)
				query.set("title", this.todo.title)
				query.set("content", this.todo.content)
				query.save().then(res => {
				  console.log("修改数据", res)
				  uni.showToast({
				  	title:"修改成功"
				  })
				  setTimeout(function() {
					  uni.navigateBack()
				  }, 500)
				}).catch(err => {
				  console.error("添加数据", err)
				  uni.showToast({
				  	title:"修改失败，请重试",
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
		margin: 20rpx 20rpx 0;
		font-size: 30rpx;
		transition: all .3s;
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
