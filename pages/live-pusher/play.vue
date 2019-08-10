<template>
	<view style="width:100%;height: 100%; background-color: black;">
		<view style="width:100%;height:100%;">
			<live-player style="width:100%;height:100%; position:absolute;" :id="video-livePlayer" :mode="live" :orientation="orientation"
			 :muted="muted" :background-mute="backgroundMute" :object-fit="objectFit" min-cache="1" max-cache="3" :src="playUrl"
			 :debug="debug" bindstatechange="onPlayEvent" bindfullscreenchange="onFullScreenChange">


				<cover-image :hidden="playing" style="width:100%; height: 100%; position:absolute;" src="https://mc.qcloudimg.com/static/img/7da57e0050d308e2e1b1e31afbc42929/bg.png">
				</cover-image>

				<cover-view style="width:100%; height: 70px; position:absolute;bottom:30px;">
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+ [playing ?'suspend':'start']+'.png'" @tap='onPlayClick'></cover-image>
						<cover-view class='bottom_text'>播放</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" src="../../static/images/qr.png" @tap='onScanQR'></cover-image>
						<cover-view class='bottom_text'>扫码</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" src="../../static/images/rotate.png" @tap='onOrientationClick'></cover-image>
						<cover-view class='bottom_text'>{{orientation == 'vertical'?'竖屏':'横屏'}}</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+ [debug ?'log':'log2']+'.png'" @tap='onLogClick'></cover-image>
						<cover-view class='bottom_text'>日志</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+[muted?'sound-dis':'sound']+'.png'" @tap='onMuteClick'></cover-image>
						<cover-view class='bottom_text'>声音</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+ [objectFit == 'contain'?'adjust':'fill']+'.png'" @tap='onObjectfitClick'></cover-image>
						<cover-view class='bottom_text'>{{objectFit == 'contain'?'适应':'填充'}}</cover-view>
					</cover-view>
				</cover-view>
				<cover-image class='close' :style="[{top:headerHeight + statusBarHeight - 26+'rpx'}]" src="../../static/images/back.png"
				 @tap="onBack"></cover-image>
			</live-player>
		</view>
	</view>
</template>

<script>
	const app = getApp()
	export default {
		data() {
			return {
				playing: false,
				videoContext: {},

				fullScreen: false,
				playUrl: "http://5815.liveplay.myqcloud.com/live/5815_89aad37e06ff11e892905cb9018cf0d4_550.flv",
				orientation: "vertical",
				objectFit: "contain",
				muted: false,
				backgroundMuted: false,
				debug: false,

				headerHeight: app.globalData.headerHeight,
				statusBarHeight: app.globalData.statusBarHeight,
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
			this.createContext();
			console.log(this.videoContext);
			uni.setKeepScreenOn({
				keepScreenOn: true,
			})
		},

		/**
		 * 生命周期函数--监听页面显示
		 */
		onShow: function() {
			// 保持屏幕常亮
			uni.setKeepScreenOn({
				keepScreenOn: true,
			})
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
			this.stop();
			uni.setKeepScreenOn({
				keepScreenOn: true,
			})
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
			return {
				// title: '直播播放器',
				// path: '/pages/play/play',
				path: '/pages/home-page/main',
				imageUrl: 'https://mc.qcloudimg.com/static/img/dacf9205fe088ec2fef6f0b781c92510/share.png'
			}
		},

		/**
		 * 用户点击右上角分享
		 */
		onShareAppMessage: function() {
			console.log("onLoad onShareAppMessage");
			return {
				// title: 'RTMP推流',
				// path: '/pages/push/push',
				path: '/pages/home-page/main',
				imageUrl: 'https://mc.qcloudimg.com/static/img/dacf9205fe088ec2fef6f0b781c92510/share.png'
			}
		},

		methods: {

			onScanQR: function() {
				this.stop();
				this.createContext();
				var self = this;
				uni.scanCode({
					onlyFromCamera: true
					// success: (res) => {
					// 	console.log(res);
					// 	self.setData({
					// 		playUrl: res.result
					// 	})
					// }
				})
			},

			onPlayClick: function() {
				var url = this.playUrl;
				if (url.indexOf("rtmp:") == 0) {} else if (url.indexOf("https:") == 0 || url.indexOf("http:") == 0) {
					if (url.indexOf(".flv") != -1) {}
				} else {
					uni.showToast({
						title: '播放地址不合法，目前仅支持rtmp,flv方式!',
						icon: 'loading',
					})
				}
					this.playing=!this.playing
				if (this.playing) {
					this.videoContext.play();
					console.log("video play()");
					wx.showLoading({
						title: '',
					})
				} else {
					this.videoContext.stop();
					console.log("video stop()");
					wx.hideLoading();
				}
			},

			onOrientationClick: function() {
				if (this.orientation == "vertical") {
					this.orientation = "horizontal";
				} else {
					this.orientation = "vertical";
				}

					this.orientation= this.orientation
			},

			onObjectfitClick: function() {
				if (this.objectFit == "fillCrop") {
					this.objectFit = "contain";
				} else {
					this.objectFit = "fillCrop";
				}
					this.objectFit= this.objectFit
				
			},

			onLogClick: function() {
				this.debug= !this.debug
				var self = this;
				setTimeout(function() {
					this.exterFlag= !self.exterFlag
				}, 10);
			},

			onMuteClick: function() {
					this.muted=!this.muted
			},

			onFullScreenClick: function() {

				if (!this.fullScreen) {
					this.videoContext.requestFullScreen({
						direction: 0,

					})

				} else {
					this.videoContext.exitFullScreen({

					})
				}
			},
			onPlayEvent: function(e) {
				console.log(e.detail.code);
				if (e.detail.code == -2301) {
					this.stop();
					uni.showToast({
						title: '拉流多次失败',
					})
				}
				if (e.detail.code == 2004) {
					uni.hideLoading();
				}
			},

			onFullScreenChange: function(e) {
				this.fullScreen= e.detail.fullScreen
				console.log(e);
				uni.showToast({
					title: this.fullScreen ? '全屏' : '退出全屏',
				})
			},

			stop: function() {
				this.playing=false,
				//this.playUrl= "rtmp://2157.liveplay.myqcloud.com/live/2157_wx_live_test1",
				this.orientation= "vertical",
				this.objectFit= "contain",
				this.muted= false,
				this.fullScreen= false,
				this.backgroundMuted= false,
				this.debug= false,
				this.exterFlag= false,
				this.videoContext.stop();
				uni.hideLoading();
			},

			createContext: function() {
				this.videoContext= uni.createLivePlayerContext("video-livePlayer")
			},
			onBack: function () {
				uni.navigateBack({
				delta: 1
				});
			}

		}
	}
</script>

<style>
/* pages/play/play.wxss */
.bottom_box
{
  position: relative;
  width: 50px; 
  height: 70px; 
  margin-left:calc(100% / 12 - 25px); 
  margin-right:calc(100% / 12 - 25px);
  display:inline-block;
}

.bottom_button
{
  display:inline-block;
  position: absolute;
  width: 50px; 
  height: 50px; 
}

.bottom_button :active
{
  opacity: 0.55
}

.bottom_text
{
  position: absolute;
  display:inline-block;
  color: #ffffff;
  width: 100%;
  font-size:10px;
  bottom: 0;
  text-align: center;
}
</style>
