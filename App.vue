<script>
	import Bmob from "hydrogen-js-sdk";
	export default {
		data() {
			return  {
				globalData: {}
			}
		},
		onLaunch: function() {
			console.log('App Launch')
			Bmob.initialize("5783eb16beed4de9", "0364c7");
			getApp().Bmob = Bmob
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>	
<style>
	.container {
		font-size: 30rpx;
		position: relative;
	}
	.ui-button {
		color: white;
		margin: 20rpx;
		background: #88B04B;
		box-shadow: 1px 1px 3px rgba(136, 176, 75, .3);
	}
	.button-hover {
		box-shadow: none !important;
		background: #6c8b3b !important;
	}
</style>
