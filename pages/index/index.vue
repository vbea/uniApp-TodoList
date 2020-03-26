<template>
	<view class="container">
		<uni-swipe-action class="list-todo">
			<view class="list-title">未完成的任务</view>
			<uni-swipe-action-item v-for="(item,index) in todoList" v-if="item.completed==false" :key="item.objectId" :options="optionsMenu" @click="completeItem($event, item)">
				<view :id="index" class="item-todo" hover-class="hover-item" @click="clickItem">
					<view class="item-title">{{item.title}}</view>
					<view class="item-update">{{item.updatedAt}}</view>
				</view>
			</uni-swipe-action-item>
			<view class="no-data">没有记录</view>
			<view class="list-title">已完成的任务</view>
			<uni-swipe-action-item v-for="(item, index) in todoList" :key="item.objectId" :options="optionsMenu2" v-if="item.completed==true" @click="uncompletedItem($event, item)">
				<view :id="index" class="item-todo" hover-class="hover-item" @click="clickItem">
					<view class="item-title">{{item.title}}</view>
					<view class="item-update">{{item.updatedAt}}</view>
				</view>
			</uni-swipe-action-item>
		</uni-swipe-action>
		<view class="no-data">没有记录</view>
		<view class="btnAdd" hover-class="button-hover" @click="toAdd">+</view>
	</view>
</template>

<script>
	import {uniSwipeAction} from '@dcloudio/uni-ui'
	import {uniSwipeActionItem} from '@dcloudio/uni-ui'
	var Bmob
	export default {
		data() {
			return {
				title: 'Hello',
				todoList: [],
				pageNumber: 0,
				canRefresh: true,
				optionsMenu:[{
					text: '已完成',
					style: {
						backgroundColor: '#88B04B'
					}
				}, {
					text: '删除',
					style: {
						backgroundColor: '#dd524d'
					}
				}],
				optionsMenu2:[{
					text: '未完成',
					style: {
						backgroundColor: '#007aff'
					}
				}, {
					text: '删除',
					style: {
						backgroundColor: '#dd524d'
					}
				}]
			}
		},
		components: {
			uniSwipeAction,
			uniSwipeActionItem
		},
		onLoad() {
			Bmob = getApp().Bmob;
			console.log("onLoad");
		},
		onShow:function() {
			console.log("onShow")
			this.refreshData();
		},
		onPullDownRefresh:function() {
			this.refreshData()
		},
		onReachBottom:function() {
			if (this.canRefresh) {
				this.pageNumber += 1;
				this.getData();
			}
		},
		methods: {
			refreshData: function() {
				this.canRefresh = true;
				this.pageNumber = 0;
				this.getData();
			},
			getData: function() {
				const query = Bmob.Query('list');
				query.limit(20);
				query.skip(this.pageNumber*20)
				query.order("completed","-createdAt");
				query.find().then(res => {
					console.log("查询"+this.pageNumber, res);
					if (this.pageNumber > 0) {
						this.todoList = this.todoList.concat(res)
						this.canRefresh = (res.length == 20)
					} else {
						this.todoList = res
					}
				}).catch(err => {
					console.log(err)
				}).finally(() => {
					uni.stopPullDownRefresh()
				})
			},
			completeItem: function(e, item) {
				console.log("action", e, item)
				if (e.index == 0) {
					this.updateTodo(item)
				} else if (e.index == 1) {
					this.deleteItem(item)
				}
			},
			uncompletedItem: function(e, item) {
				if (e.index == 0) {
					this.updateTodo(item)
				} else if (e.index == 1) {
					this.deleteItem(item)
				}
			},
			deleteItem: function(item) {
				uni.showModal({
					title:"删除",
					content:"确定要删除此条数据？",
					confirmColor: "#88B04B",
					success: (res) => {
						if (res.confirm) {
							uni.showLoading({
								title:"请稍后..."
							})
							this.deleteTodo(item.objectId)
						}
					}
				})
			},
			updateTodo: function(item) {
				uni.showLoading({
					title:"请稍后..."
				})
				const query = Bmob.Query('list');
				query.set('id', item.objectId);
				query.set('completed', !item.completed);
				query.save().then(res => {
					console.log("修改数据", res)
					uni.showToast({
						title:"操作成功"
					})
					this.refreshData();
				}).catch(err => {
					console.error("修改数据", err)
					uni.showToast({
						title:"操作失败",
						icon:"none"
					})
				})
			},
			deleteTodo: function(id) {
				const query = Bmob.Query('list');
				query.destroy(id).then(res => {
				  console.log("删除数据", res)
				  uni.showToast({
				  	title:"删除成功"
				  })
				  this.refreshData();
				}).catch(err => {
				  console.error("删除数据", err)
				  uni.showToast({
				  	title:"删除失败",
					icon:"none"
				  })
				})
			},
			clickItem: function(e) {
				var item = this.todoList[Number(e.currentTarget.id)];
				getApp().globalData.itemData = item
				uni.navigateTo({
					url:"../Detail/Detail"
				})
			},
			toAdd: function(e) {
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
		right: 36rpx;
		bottom: 36rpx;
		width: 110rpx;
		height: 110rpx;
		display: flex;
		position: fixed;
		font-size: 60rpx;
		align-items: center;
		border-radius: 90rpx;
		justify-content: center;
		background: $color-accent;
		box-shadow: 1px 2px 3px rgba($color: $color-accent, $alpha: .4);
	}
</style>
<style>
	.item-todo {
		width: 100%;
		padding: 15rpx 20rpx;
	}
	
	.item-title {
		width: 100%;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}
	
	.uni-swipe + .uni-swipe {
		border-top: 1px solid #eee;
	}
	
	.hover-item {
		background: #e0e0e0;
	}
	
	.item-update {
		color: #C0C0C0;
		font-size: 25rpx;
	}
	
	.list-title {
		top: 44px;
		z-index: 3;
		font-size: 25rpx;
		background: #EEE;
		position: sticky;
		padding: 10rpx 20rpx;
	}
	
	.no-data {
		color: #ccc;
		display: none;
		padding: 50rpx;
		font-size: 25rpx;
		text-align: center;
	}
	
	.list-title + .no-data {
		display: block;
	}
</style>
