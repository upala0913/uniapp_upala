<template>
	<view class="content">
		<image class="logo" src="/static/logo/logo.jpg"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="skip" @click="skip">跳转到我的页面</view>
		<!-- <audio :src="audio.src" :poster="audio.poster" :name="audio.name" :author="audio.author" :action="audioActive.method" controls></audio> -->
		<image src="/static/testImage/shuijiao.jpg" mode="widthFix" style="width: 200rpx;"></image>
		<video :src="video.src" :danmu-list="video.danmuList" :danmu-btn="video.danmuBtn" :enable-danmu="video.enableDanmu" controls></video>
		<!-- <audio :src="myAudio.src" :poster="myAudio.poster" :name="myAudio.name" :author="myAudio.author" :action="audioActive.method" controls></audio> -->
		<view class="image" @click="choose" >点击选择图片</view>
		<view class="myAudio1" @click="myAudio1">点击播放</view>
		<image src="../../static/trumpet/trumpet.png" mode="widthFix" style="width: 100rpx;" @click="trumpetAudio" ></image>
		<view class="phone" @click="callPhone" >打电话</view>
		<view class="scanCode" @click="scanCode" >扫码</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title : '欢迎来到XXX管理APP',
				audio : {
					src : "https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.mp3",
					poster : "https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.jpg",
					name : "致爱丽丝",
					author : "暂无",
				},
				audioActive : {
					method : "pause"
				},
				video: {
					src : "https://dcloud-img.oss-cn-hangzhou.aliyuncs.com/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-"+
					"app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126.mp4",
					danmuList:[{
							text: "王鹏你好帅",
							color: "#ff0000",
							time:1
						}, {
							text:"祖国生日快乐",
							color:"#ff00ff",
							time:3
						}
					],
					danmuBtn : true,
					enableDanmu : true
				},
				myAudio: {
					src:"https://upala.oss-cn-hangzhou.aliyuncs.com/zjapp/%E8%AF%AD%E9%9F%B3.mp3",
					poster:"https://upala.oss-cn-hangzhou.aliyuncs.com/zjapp/%E8%93%9D%E5%BA%95.jpg",
					name:"语音播报",
					author:"王鹏"
				}
			}
		},
		methods : {
			skip : function() {
				uni.reLaunch({
					url:"../login/login"
				})
			},
			choose : function() {
				uni.chooseImage({
					count : 3,
					sizeType: "compressed",
					success : function(res) {
						console.log(res);
					}
				})
			},
			myAudio1 : function() {
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.loop = true;
				innerAudioContext.autoplay = true;
				innerAudioContext.src = "https://upala.oss-cn-hangzhou.aliyuncs.com/zjapp/%E8%AF%AD%E9%9F%B3.mp3";
				innerAudioContext.onPlay(() => {
					console.log("点击开始播放音乐！！！");
				});
				innerAudioContext.onError((res) => {
					console.log(res);
				});
			},
			trumpetAudio : function(e) {
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.play();
				console.log("处于停止状态，现在要播放！！！");
				innerAudioContext.loop = true;
				innerAudioContext.src = "https://upala.oss-cn-hangzhou.aliyuncs.com/zjapp/%E6%98%A5%E6%B1%9F%E8%8A%B1%E6%9C%88%E5%A4%9C.mp3";
				innerAudioContext.onPlay(function(){
					console.log("开始播放春江花月夜！！！");
				})
				innerAudioContext.onError(function(res){
					console.log(res);
				});
			},
			callPhone : function() {
				uni.makePhoneCall({
					phoneNumber : "13920742598"
				})
			},
			scanCode : function() {
				uni.scanCode({
					success : function(res) {
						console.log(res);
					}
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
