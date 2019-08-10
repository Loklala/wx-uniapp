<template>
	<view class="content">
		<view class="mainUI">
			<view class='title' :style="[{paddingTop:titleheight + 'px'}]">
				<text>RTMP推流</text>
			</view>
			<view class='item' style='padding-top:2vh'>
				<text class='item-title'>推流获取</text>
				<button class="item-button1" @tap="onScanQR()">扫码读取</button>
				<button class="item-button1" @tap="onNewUrlClick()">自动生成</button>
			</view>
			<view class='item'>
				<text class='item-title'>推流地址</text>
				<input class='item-input' placeholder="rtmp://" @focus="focusPush()" @tap='onPushInputTap()' @input="onPushInputChange()"
				 :value="pushUrl"></input>
				<image class='item-image' @tap="onCopyPushUrl" src='../../../static/images/copy.png'></image>
			</view>
			<view class='item'>
				<text class='item-title'>播放地址</text>
				<input class='item-input' placeholder="http://" @focus="focusPlay()" @tap='onPlayInputTap()' @input="onPlayInputChange()"
				 :value="playUrl"></input>
				<image class='item-image' @tap="onCopyPlayUrl()" src='../../../static/images/copy.png'></image>
			</view>
			<view class='item'>
				<text class='item-title'>画面质量</text>
				<radio-group class="radio-group" @change="modeRadioChange">
					<label class="radio" v-for="(item,index) in modeItems" :class="mode == item.value ? 'bc_blue white': 'blue'">
						<radio :value="item.value" :checked="item.checked" />
						{{item.title}}
					</label>
				</radio-group>
			</view>
			<view class='item'>
				<text class='item-title'>画面方向</text>
				<radio-group class="radio-group" @change="orientationRadioChange">
					<label class="radio" v-for="(item,index) in orientationItems" :class="orientation == item.value ? 'bc_blue white': 'blue'">
						<radio :value="item.value" :checked="item.checked" />
						{{item.title}}
					</label>
				</radio-group>
			</view>
			<view class='item'>
				<text class='item-title'>仅推声音</text>
				<switch class='item-switch' :checked="!enableCamera" @change="switchChange(this)" />
			</view>
		</view>

		<view class='start-box'>
			<button class='start' @tap="startPush()" src='../../../static/images/logo.png'>开始</button>
		</view>

		<view class='logo-box'>
			<image class='logo' src='../../../static/images/logo.png'></image>
		</view>

		<cover-image class='close' :style="[{top:headerHeight + statusBarHeight -26+ 'rpx'}]" src="../../../static/images/back.png"
		 @tap="onBack()"></cover-image>
	</view>
</template>

<script>
	const app = getApp()
	export default {
		data() {
			return {
				modeItems: [{
						value: 'SD',
						title: 'SD'
					},
					{
						value: 'HD',
						title: 'HD'
					},
					{
						value: 'FHD',
						title: 'FHD'
					},
				],
				mode: 'SD',
				orientationItems: [{
						value: 'vertical',
						title: '竖屏推流'
					},
					{
						value: 'horizontal',
						title: '横屏推流'
					},
				],
				orientation: 'vertical',
				enableCamera: true,
				focusPush: false,
				focusPlay: false,
				tapTime: '',
				headerHeight: app.globalData.headerHeight, //
				statusBarHeight: app.globalData.statusBarHeight,
				titleheight: (this.headerHeight + this.statusBarHeight) / 2 - 12,
				
				pushUrl:'',
				playUrl:'',
			}
		},
		/**
		 * 生命周期函数--监听页面加载
		 */
		onLoad: function(options) {

		},

		/**
		 * 生命周期函数--监听页面初次渲染完成
		 */
		onReady: function() {

		},

		/**
		 * 生命周期函数--监听页面显示
		 */
		onShow: function() {

		},

		/**
		 * 生命周期函数--监听页面隐藏
		 */
		onHide: function() {

		},

		/**
		 * 生命周期函数--监听页面卸载
		 */
		onUnload: function() {

		},

		/**
		 * 页面相关事件处理函数--监听用户下拉动作
		 */
		onPullDownRefresh: function() {

		},

		/**
		 * 页面上拉触底事件的处理函数
		 */
		onReachBottom: function() {

		},

		/**
		 * 用户点击右上角分享
		 */
		onShareAppMessage: function() {

		},
		
		methods: {
			onBack: function() {
				uni.navigateBack({
					delta: 1
				});
			},
			onPushInputTap: function() {
					this.focusPush=true
			},
			onPushInputChange: function(e) {
					this.pushUrl=e.detail.value
			},
			onPlayInputTap: function() {
					this.focusPlay= true
			},
			onPlayInputChange: function(e) {
				console.log(e)
				this.playUrl= e.detail.value
			},
			modeRadioChange: function(e) {
				console.log(e)
				this.mode=e.detail.value;
			},
			orientationRadioChange: function(e) {
				console.log(e)
				this.orientation= e.detail.value
			},
			switchChange: function(e) {
				console.log(e)
				this.enableCamera= !e.detail.value
			},
			onScanQR: function() {
				var self = this;
				uni.scanCode({
					onlyFromCamera: true,
					// success: (res) => {
					// 	console.log(res);
					// 		self.pushUrl= res.result,
					// 		self.playUrl= "",
					// }
				})
			},

			onNewUrlClick: function() {
				this.pushUrl= 'rtmp://58466.livepush.myqcloud.com/live/e56d79882d?bizid=58466&txSecret=a4081f7bcdc5d7bb85efd2eb25a14c34&txTime=5D4EE9FF'
// 				var self = this;
// 				uni.request({
// 					url: '58466.livepush.myqcloud.com',
// 					data:{
// 						
// 					},
// 					success: (res) => {
// 						console.log(res);
// 						var pushUrl = res.data['url_push'];
// 						var rtmpUrl = res.data['url_play_rtmp'];
// 						var flvUrl = res.data['url_play_flv'];
// 						var hlsUrl = res.data['url_play_hls'];
// 						var accUrl = res.data['url_play_acc'];
// 						console.log(pushUrl);
// 						
// 							this.pushUrl= pushUrl,
// 							this.playUrl= flvUrl,
// 
// 						wx.showToast({
// 							title: '获取地址成功',
// 						})
// 					},
// 					fail: (res) => {
// 						console.log(res);
// 						wx.showToast({
// 							title: '网络或服务器异常',
// 						})
// 					}
// 				})
			},
			onCopyPushUrl: function() {
				// wx.setClipboardData({
				// 	data= this.pushUrl
				// })
			},
			onCopyPlayUrl: function() {
				// wx.setClipboardData({
				// 	data= this.playUrl
				// })
			},
			startPush: function() {
				var self = this;
				// 防止两次点击操作间隔太快
				var nowTime = new Date();
				if (nowTime - this.tapTime < 1000) {
					return;
				}
				if (!self.pushUrl || self.pushUrl.indexOf("rtmp://") != 0) {
					wx.showModal({
						title: '提示',
						content: '推流地址不合法，请点击自动生成按钮先获取腾讯云推流地址',
						showCancel: false
					});
					return;
				}
				var url = '/pages/live-pusher/push?pushUrl=' + encodeURIComponent(self.pushUrl) + '&mode=' + self.mode +
					'&orientation=' + self.orientation + '&enableCamera=' + self.enableCamera;
				console.log(url);
				uni.navigateTo({
					url: url
				});
				self.tapTime= nowTime
			},

		},
	}
</script>

<style>
	/* pages/push-config/push-config.wxss */
	.item {
		width: 88vw;
		height: 30px;
		padding-left: 6vw;
		margin-top: 10px;
		padding-bottom: 10px;
		display: flex;
		flex-flow: row;
		align-items: center;
	}

	.item-title {
		width: 20vw;
		height: 20px;
		font-size: 14px;
		color: #ffffff;
	}

	.item-button1 {
		display: flex;
		width: 23vw;
		height: 30px;
		padding: 0 0 0;
		margin-left: 2vw;
		margin-right: 2vw;
		font-size: 14px;
		color: #000000;
		justify-content: center;
		align-items: center;
	}

	.item-input {
		width: 50vw;
		height: 30px;
		margin-left: 2vw;
		font-size: 14px;
		color: #000000;
		background-color: #ffffff;
		border-radius: 8rpx;
	}

	.item-image {
		margin-left: 3vw;
		width: 10vw;
		height: 10vw;
	}

	.radio-group {
		background-color: white;
		display: flex;
		width: 50vw;
		margin-left: 2vw;
		text-align: center;
		font-size: 14px;
		border: 1px solid #0076FF;
		border-radius: 5px;
	}

	.radio-group radio {
		display: none;
	}

	.blue {
		color: #0076FF;
	}

	.bc_blue {
		background-color: #0076FF;
	}

	.white {
		color: white;
	}

	label {
		text-align: center;
		padding: 3px 0;
		flex: 1;
		border-right: 1px solid #0076FF;
	}

	label:last-child {
		border-right: 0;
	}

	.item-switch {
		margin-left: 2vw;
	}

	.start-box {
		position: absolute;
		width: 100vw;
		bottom: 150rpx;
		text-align: center;
	}

	.start {
		display: flex;
		width: 20vh;
		height: 20vh;
		font-size: 20px;
		color: #0080FF;
		border-radius: 10vh;
		justify-content: center;
		align-items: center;
	}
</style>
