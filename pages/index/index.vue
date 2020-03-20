<template>
	<view class="content">
		<view class="list-todo">
			<view id="item.objectId" class="item-todo" hover-class="hover-item" v-for="item in todoList" v-bind:key="item.objectId">
				<view class="item-title">{{item.content}}</view>
				<view class="item-update">{{item.updatedAt}}</view>
			</view>
		</view>
		<view class="btnAdd" @click="toAdd">+</view>
	</view>
</template>

<script>
	import Bmob from "hydrogen-js-sdk";
	import moment from "moment";
	export default {
		data() {
			return {
				title: 'Hello',
				todoList: []
			}
		},
		onLoad() {
			Bmob.initialize("5783eb16beed4de9", "0364c7");
			//Bmob.debug(true);
			let a = 100;
			console.log("onLoad", moment().format("YYYY-MM-DD HH:mm"));
		},
		onShow:function(){
			const query = Bmob.Query('list');
			query.find().then(res => {
			  console.log("查询", res)
			  this.todoList = res
			  console.log(this.todoList.length)
			}).catch(err => {
			  console.log(err)
			})
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
<style lang="scss">
	.btnAdd {
		z-index: 20;
		color: white;
		width: 90rpx;
		right: 36rpx;
		bottom: 36rpx;
		height: 90rpx;
		display: flex;
		position: fixed;
		font-size: 55rpx;
		align-items: center;
		border-radius: 90rpx;
		justify-content: center;
		background: $color-accent;
		box-shadow: 1px 2px 3px rgba($color: $color-accent, $alpha: .4);
	}
</style>
<style>
	.content {
		font-size: 30rpx;
	}
	
	.item-todo {
		padding: 15rpx 20rpx;
	}
	
	.item-todo + .item-todo {
		border-top: 1px solid #eee;
	}
	
	.hover-item {
		background: #e0e0e0;
	}
	
	.item-update {
		color: #C0C0C0;
		font-size: 25rpx;
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
