<template>
	<view class="content">
		<view class="mainUI"> 
			<view class='title' :style="[{paddingTop:titleheight + 'px'}]">
				<text>lala视频云</text>
			</view>
  <view class='tips'>
    <text>以下将展示小程序互动音视频能力，由腾讯云提供技术支持</text>
  </view>
  <view class='guide-box'>
    <view  v-for="(item,index) in entryInfos"  class='guide' @click="pageTo(item.navigateTo)" :data-url="item.navigateTo">
      <block>
        <image class="guide_icon" :src="item.icon" mode="aspectFit" />
        <view class="guide_label">{{item.title}}</view>
        <view class="guide_desc">{{item.desc}}</view>
      </block>
    </view>
    <view class='guide-nomore guide-nomore-bk'>
      <view class="guide_nomore_label">
        <view>更多功能</view>
        <view>敬请期待</view> 
      </view>
    </view>
  </view>
  <view>
	  <view class='logo-box'>
    <image class='logo' src='../../static/images/logo.png'></image>
  </view>
  </view>
</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				canShow: 0,
				tapTime: '',		// 防止两次点击操作间隔太快
				entryInfos: [
				{ icon: "../../static/images/play.png", 
					title: "手机直播", 
					desc: "<mlvb-live-room>",
					 navigateTo: "../mlvb-live-room/roomlist",
					 },
				{ icon: "../../static/images/push.png", 
					title: "RTMP推流", 
					desc: "<live-pusher>",
					 navigateTo: "../live-pusher/push-config/push-config" ,
					 },
				{ icon: "../../static/images/play.png",
					title: "直播播放", 
					desc: "<live-player>", 
					navigateTo: "../live-pusher/play" ,
					},
				{ icon: "//", //../../static/images/rtplay.png
					title: "", 
					desc: "", //<live-player>
					navigateTo: "",//../rtc-player-demo/rtplay
					}
			
				],
				headerHeight:getApp().globalData.headerHeight, //
				statusBarHeight:getApp().globalData.statusBarHeight,
				titleheight:(this.headerHeight+this.statusBarHeight)/2-12
			}
		},
		/**
		* 生命周期函数--监听页面初次渲染完成
		*/
		onReady: function () {
			console.log("onReady");
			if(!wx.createLivePlayerContext) {
			setTimeout(function(){
				wx.showModal({
				title: '提示',
				content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后再试。',
				showCancel: false
				});
			},0);
			} else {
				console.log(getApp().globalData.headerHeight);
			// 版本正确，允许进入
			// this.data.canShow = 1;
			}
		},
		/**
		* 生命周期函数--监听页面显示
		*/
		onShow: function () {
			console.log("onShow");
		
		},
		
		/**
		* 生命周期函数--监听页面隐藏
		*/
		onHide: function () {
			console.log("onHide");
		
		},
		
		/**
		* 生命周期函数--监听页面卸载
		*/
		onUnload: function () {
			console.log("onUnload");
		
		},
		
		/**
		* 页面相关事件处理函数--监听用户下拉动作
		*/
		onPullDownRefresh: function () {
			console.log("onPullDownRefresh");
		
		},
		
		/**
		* 页面上拉触底事件的处理函数
		*/
		onReachBottom: function () {
			console.log("onReachBottom");
		
		},
		
		/**
		* 用户点击右上角分享
		*/
		onShareAppMessage: function () {
			console.log("onShareAppMessage");
			return {
			title: '腾讯视频云',
			path: '/pages/home-page/main',
			imageUrl: 'https://mc.qcloudimg.com/static/img/dacf9205fe088ec2fef6f0b781c92510/share.png'
			}
		},
		onLoad() {

		},
		methods: {
			pageTo:function(e){
				console.log(e);
				uni.navigateTo({
						url:e
					})
			},
			onEntryTap: function (e) {
				if (this.data.canShow) {
				// if(1) {
				// 防止两次点击操作间隔太快
				var nowTime = new Date();
				if (nowTime - this.data.tapTime < 1000) {
					return;
				}
				var toUrl = this.data.entryInfos[e.currentTarget.id].navigateTo;
				console.log(toUrl);
				wx.navigateTo({
					url: toUrl,
				});
				this.setData({ 'tapTime': nowTime });
				} else {
				wx.showModal({
					title: '提示',
					content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后再试。',
					showCancel: false
				});
				}
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
	}

	.logo {
		height: 60upx;
		width: 160upx;
		margin-top: 200upx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50upx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
</style>
