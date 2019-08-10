<template>
	<view style="width:100%;height: 100%; background-color: black;">
		<view style="width:100%;height:100%;">
			<live-pusher style="width:100%; height: 100%;"
			 :id="camera-push" 
			 :mode="mode" 
			 :orientation="orientation" 
			 :beauty="beauty" 
			 :whiteness="whiteness" 
			 :muted="muted" 
			 waiting-image="https://mc.qcloudimg.com/static/img/daeed8616ac5df256c0591c22a65c4d3/pause_publish.jpg" 
			 bindstatechange="onPushEvent">
				<cover-view style="width:100%; height: 70px; position:absolute;bottom:30px;">
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" src="../../static/images/camera.png" @tap='onSwitchCameraClick()'></cover-image>
						<cover-view class='bottom_text'>翻转</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+[beauty == 0 ? 'beauty-dis':'beauty']+'.png'" @tap='onBeautyClick'></cover-image>
						<cover-view class='bottom_text'>美颜</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+[muted ? 'mic-dis':'mic']+'.png'" @tap='onMuteClick'></cover-image>
						<cover-view class='bottom_text'>声音</cover-view>
					</cover-view>
					<cover-view class='bottom_box'>
						<cover-image class="bottom_button" :src="'../../static/images/'+[debug?'log':'log2']+'.png'" @tap='onLogClick()'></cover-image>
						<cover-view class='bottom_text'>日志</cover-view>
					</cover-view>
				</cover-view>
				<cover-image class='close' :style="[{top:headerHeight + statusBarHeight - 26+'rpx'}]" src="../../static/images/back.png"
				 @tap="onBack"></cover-image>
			</live-pusher>

		</view>
	</view>
</template>

<script>
	const app = getApp()
	export default {
		data() {
			return {
				frontCamera: true,
				cameraContext: {},
				pushUrl: "",
				mode: "SD",
				muted: false,
				enableCamera: true,
				orientation: "vertical",
				beauty: 6.3,
				whiteness: 3.0,
				backgroundMute: false,
				debug: false,
				headerHeight: app.globalData.headerHeight,
				statusBarHeight: app.globalData.statusBarHeight,
			}
		},
		/**
		 * 生命周期函数--监听页面加载
		 */
		onLoad: function(options) {
			console.log("onLoad");
			this.mode=options.mode
			this.orientation= options.orientation
			this.enableCamera= options.enableCamera === "false" ? false : true
			this.pushUrl= decodeURIComponent(options.pushUrl)
		},
		/**
		 * 生命周期函数--监听页面初次渲染完成
		 */
		onReady: function() {
			console.log("onLoad onReady");
			this.createContext();

			uni.setKeepScreenOn({
				keepScreenOn: true,
			})
		},

		/**
		 * 生命周期函数--监听页面显示
		 */
		onShow: function() {
			console.log("onLoad onShow");
			// 保持屏幕常亮
			wx.setKeepScreenOn({
				keepScreenOn: true
			})
		},

		/**
		 * 生命周期函数--监听页面隐藏
		 */
		onHide: function() {
			console.log("onLoad onHide");

		},

		/**
		 * 生命周期函数--监听页面卸载
		 */
		onUnload: function() {
			console.log("onLoad onUnload");
			this.stop();

			wx.setKeepScreenOn({
				keepScreenOn: false,
			})
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
			onBack: function() {
				uni.navigateBack({
					delta: 1
				});
			},
			onSwitchCameraClick: function() {
				this.frontCamera = !this.frontCamera;
				this.frontCamera=this.frontCamera
				this.cameraContext.switchCamera();
			},
			onBeautyClick: function() {
				if (this.beauty != 0) {
					this.beauty = 0;
					this.whiteness = 0;
				} else {
					this.beauty = 6.3;
					this.whiteness = 3.0;
				}

					this.beauty= this.beauty,
					whiteness= this.whiteness
			},
			onLogClick: function() {
					this.debug=!this.debug
			},
			onMuteClick: function() {
					this.muted= !this.muted
			},
			onPushEvent: function(e) {
				uni.showModal({
					content: e.detail.code,
					showCancel: false
				});
				console.log(e.detail.code);
				if (e.detail.code == -1307) {
					this.stop();
					wx.showToast({
						title: '推流多次失败',
					})
				}
			},

			stop: function() {
					this.playing= false,
					this.pushUrl= "",
					this.mode= "SD",
					this.muted= false,
					this.enableCamera= true,
					this.orientation= "vertical",
					this.beauty= 6.3,
					this.whiteness= 3.0,
					this.backgroundMute= false,
					this.debug= false
				this.cameraContext.stop();
			},

			createContext: function() {
				// var self = this;
					this.cameraContext=uni.createLivePusherContext('camera-push'),
					this.cameraContext.start();
			},

		}
	}
</script>

<style>
	/* pages/push/push.wxss */

.bottom_box
{
  position: relative;
  width: 50px; 
  height: 70px; 
  margin-left:calc(100% / 8 - 25px); 
  margin-right:calc(100% / 8 - 25px);
  display:inline-block;
}

.bottom_button
{
  display:inline-block;
  position: absolute;
  width: 50px; 
  height: 50px; 
}

.bottom_button :active{
  opacity:0.55;
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
