<template>
	<view>
		<view class="title">{{title}}</view>
		<picker :range="years" @change="yearChange">
			<view>{{years[yearIndex]}}</view>
		</picker>
		<view class="newsList" v-for="(item, index) in newList" :key="index" >
			<view>{{item.title}}</view>
		</view>
		<button type="primary" @touchstart="start" @touchend="end" >语音识别</button>
		<view class="text">识别类容:{{text}}</view>
		<view class="tts" @click="tts" >语音合成</view>
	</view>
</template>

<script>
	var _self;
	export default {
		data() {
			return {
				title:"请登录",
				years:["请选择年份", 1991, 1992, 1993],
				yearIndex:0,
				newList:[],
				text : ""
			}
		},
		onLoad: function() {
			_self = this;
		},
		onPullDownRefresh : function () {
			this.getNews();
		},
		methods: {
			yearChange:function(e) {
				console.log(e.detail.value);
				this.yearIndex = e.detail.value;
			},
			getNews : function() {
				// uni.showNavigationBarLoading();
				uni.request({
					url: "https://unidemo.dcloud.net.cn/api/news",
					method: "get",
					success: res => {
						console.log(res);
						uni.hideNavigationBarLoading();
						_self.newList = res.data;
						uni.stopPullDownRefresh();
					},
					fail : function (res) {
						console.log("加载失败！！！");
					}
				})
			},
			start : function() {
				var options = {};
				options.engine = 'iFly';
				_self.text = "";
				console.log("开始识别！！！");
				plus.speech.startRecognize(options, function(s) {
					_self.text += s;
				}, function(e) {
					console.log("语音识别失败："+e)
				})
			},
			end : function() {
				plus.speech.stopRecognize();
				console.log("结束识别！！！");
			},
			tts : function() {
				var main = plus.android.runtimeMainActivity();
				var SynthesizerPlayer = plus.android.importClass('com.iflytek.speech.SynthesizerPlayer');
				var play = SynthesizerPlayer.createSynthesizerPlayer(main,'appid=__UNI__71B6A0E');
				play.playText('水果',null,null);
				
				// var receiver;
				// receiver = plus.android.implements('com.iflytek.cloud.SynthesizerListener', {
				// 	onEvent: function(eventType, arg1, arg2, obj) {
				// 		console.log("onEvent");
				// 	},
				// 	onSpeakBegin: function() {
				// 		console.log("开始阅读");
				// 	},
				// 	onSpeakPaused: function() {
				// 		console.log(" 暂停播放 ");
				// 	},
				// 	onSpeakResumed: function() {
				// 		console.log("继续播放");
				// 	},
				// 	onBufferProgress: function(percent, beginPos, endPos, info) {
				// 		console.log("合成进度" + percent);
				// 	},
				// 	onSpeakProgress: function(percent, beginPos, endPos) {
				// 		console.log("播放进度" + percent);
				// 	},
				// 	onCompleted: function(error) {
				// 		console.log("播放完毕");
				// 	}
				// });
				// var main = plus.android.runtimeMainActivity();
				// var SpeechUtility = plus.android.importClass('com.iflytek.cloud.SpeechUtility');
				// SpeechUtility.createUtility(main, "appid=__UNI__71B6A0E");
				// var SynthesizerPlayer = plus.android.importClass('com.iflytek.cloud.SpeechSynthesizer');
				// var play = SynthesizerPlayer.createSynthesizer(main, null);
				//  // 开始合成
				// play.startSpeaking('新年快乐', receiver);
				// // 取消合成
				// play.stopSpeaking();
				// // 暂停播放
				// play.pauseSpeaking();
				// // 继续播放
				// play.resumeSpeaking();
			}
		}
	}
</script>

<style>

</style>
