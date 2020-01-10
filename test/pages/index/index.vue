<template>
	<view class="content">
		<video id="myVideo" :src="tempVideoPath[currentVideo]" @ended="changeVideo()"></video>
		<view class="notice">
			{{motion[currentVideo]}}
		</view>
		<button class="but" :disabled="disabled" type="primary" @click="yqz()">录制视频</button>
		<view class="restart" @click="restart()">
			重新录像
		</view>
		<view class="upload" @click="open()">
			上传视频
		</view>
		<uni-popup ref="popup" type="center">
			<view class="popup">
				<input class="uni-input" v-model="number" placeholder="请输入学号" />
				<input class="uni-input" v-model="user" placeholder="请输入姓名" />
				<picker class="picker" mode="selector" @change="bindPickerChange" :value="index" :range="schoolList">
					<view>{{schoolList[index]}}</view>
				</picker>
				<view class="upload" @click="upload()">
					确认上传
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import App from "../../App.vue"
	import {
		uniPopup
	} from '@dcloudio/uni-ui'
	export default {
		components: {
			uniPopup
		},
		data() {
			return {
				tempThumbPath: [],
				tempVideoPath: [],
				tenVideos: [],
				result: {},
				src: "",
				alpha: "",
				b: "",
				c: "",
				disabled: false,
				ctx: {},
				currentVideo: 0,
				videoContext: {},
				motion: ["点头视频", "张嘴视频", "眨眼视频", "摇头视频", "保持不动视频"],
				number: "",
				user: "",
				schoolList: ["华南农业大学", "华南理工大学", "暨南大学"],
				school: "华南农业大学",
				index: 0
			}
		},
		onLoad() {
			var that = this
			that.ctx = uni.createCameraContext()
			that.videoContext = uni.createVideoContext('myVideo')
			console.log(that.videoContext)
			that.detect()
		},
		onShow() {
			var that = this
			that.detect()
		},
		methods: {
			bindPickerChange: function(e) {
				var that = this
				console.log(e)
				console.log('picker发送选择改变，携带值为', e.target.value)
				var ind = e.target.value
				that.index = ind
				that.school = that.schoolList[ind]
			},
			yqz: function() {
				var that = this
				uni.stopAccelerometer()
				uni.navigateTo({
					url: "camera"
				})
			},
			error(e) {
				console.log(e.detail)
			},
			set_tempVideoPath: function(temp) {
				var that = this
				that.tempVideoPath = JSON.parse(temp)
				console.log(that.tempVideoPath.length)
			},
			set_tenVideos: function(temp) {
				var that = this
				that.tenVideos = JSON.parse(temp)
				console.log(that.tenVideos.length)
			},
			changeVideo: function() {
				var that = this
				if (that.currentVideo < that.tempVideoPath.length) {
					that.currentVideo++
				} else {
					that.currentVideo = 0
				}
				console.log(that.currentVideo)
				that.videoContext.autoplay = 'true' //设置自动播放
				setTimeout(() => {
					that.videoContext.play() //播放视频
				}, 1000)
			},
			detect: function() {
				var that = this
				console.log("fuxk")
				uni.onAccelerometerChange(function(res) {
					console.log("1233")
					that.alpha = parseInt(Math.abs(res.x * 10))
					that.b = parseInt(Math.abs(res.y * 10))
					that.c = parseInt(Math.abs(res.z * 10))
					if (that.alpha >= 9 && that.b <= 2 && that.c <= 2) {
						uni.showToast({
							title: "可以开始录制视频",
							icon: "none"
						})
						that.disabled = false
					} else {
						uni.showToast({
							title: "检测到手机未横屏",
							icon: "none"
						})
						that.disabled = true
					}
				})
			},
			uploadimg: function() {
				var that = this
				uni.chooseVideo({
					count: 1,
					sourceType: ['camera', 'album'],
					success: function(res) {
						uni.uploadFile({
							url: "http://149.28.73.240/api/postFiles?studentName=werew&studentNo=3423&actionName=" + that.motion[0] +
								"&School=华南农业大学", //服务器接口
							method: 'POST', //这句话好像可以不用
							filePath: res.tempFilePath,
							header: {
								"Content-Type": "multipart/form-data"
							},
							name: 'file', //服务器定义的Key值
							success: function(res) {
								console.log(res)
							},
							fail: function(res) {
								console.log(res)
							}
						})
					}
				})
			},
			upload: function() {
				var that = this
				console.log(that.user)
				console.log(that.number)
				console.log(that.school)
				console.log(that.tempVideoPath[0])
				if (that.user.length > 0 && that.number.length > 0) {
					uni.showLoading({
						title: "正在上传"
					})
					that.uploadTenVideo()
					for (let i = 0; i < 5; i++) {
						uni.uploadFile({
							url: "http://149.28.73.240/api/postFiles?studentName=" + that.user +
								"&studentNo=" + that.number + "&actionName=" + that.motion[i] + "&School=" + that.school, //服务器接口
							// url:"http://localhost:54473/api/Values/postFiles?userName=" + that.user + "&type=" + that.motion[i] 
							// 	+ "&school=" + that.school,
							method: 'POST', //这句话好像可以不用
							filePath: that.tempVideoPath[i],
							header: {
								"Content-Type": "multipart/form-data"
							},
							name: 'file', //服务器定义的Key值
							success: function(res) {
								console.log(res)
							},
							fail: function(res) {
								console.log(res)
								uni.showToast({
									title: "视频上传失败",
									icon: "none"
								})
							}
						})
					}
				} else {
					uni.showToast({
						title: "请输入姓名或学号",
						icon: "none"
					})
				}
			},
			uploadTenVideo: function() {
				var that = this
				let k = 0
				for (let i = 0; i < 10; i++) {
					k++
					uni.uploadFile({
						url: "http://149.28.73.240/api/postCombineFiles?studentName=" + that.user +
							"&studentNo=" + that.number + "&School=" + that.school + "&videoNo=" + k, //服务器接口
						method: 'POST', //这句话好像可以不用
						filePath: that.tenVideos[i],
						header: {
							"Content-Type": "multipart/form-data"
						},
						name: 'file', //服务器定义的Key值
						success: function(res) {
							console.log(res)
							if (i == 9) {
								uni.hideLoading()
								that.$refs.popup.close()
								uni.showToast({
									title: "上传完成"
								})
							}
						},
						fail: function(res) {
							console.log(res)
							uni.showToast({
								title: "视频上传失败",
								icon: "none"
							})
						}
					})
				}
			},
			restart: function() {
				var that = this
				that.tempVideoPath = []
				uni.showToast({
					title: "已重置视频,请重新录像"
				})
			},
			open: function() {
				var that = this
				this.$refs.popup.open()
			}
		}
	}
</script>

<style>
	.green {
		margin: auto;
		height: 100upx;
		width: 100upx;
		border: 2upx solid #00ff00;
	}

	.notice {
		color: #ff0000;
	}

	.content {
		text-align: center;
		height: 100%;
		display: flex;
		flex-direction: column;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 100upx;
	}

	.title {
		display: flex;
		justify-content: space-around;
		width: 100%;
		font-size: 36upx;
		color: #8f8f94;
	}

	.contents {
		display: flex;
		height: 200upx;
		width: 100%;
		justify-content: space-around;
	}

	.but {
		height: 100upx;
		width: 70%;
		margin: auto;
	}

	.upload {
		height: 100upx;
		width: 70%;
		background: #0000FF;
		margin: auto;
		border-radius: 20upx;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 40upx;
		color: #FFFFFF;
	}

	.restart {
		height: 100upx;
		width: 70%;
		background: #FF0000;
		margin: auto;
		border-radius: 20upx;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 40upx;
		color: #FFFFFF;
	}

	#myVideo {
		width: 100%;
		height: 50%;
		margin: auto;
	}

	.uni-input {
		margin: auto;
		width: 90%;
		height: 100upx;
		background: #f2f2f2;
		border-radius: 70upx;
	}

	.popup {
		background: #FFFFFF;
		width: 600upx;
		height: 600upx;
		display: flex;
		flex-direction: column;
		border-radius: 30upx;
	}

	.picker {
		margin: auto;
		border-bottom: solid 1rpx #bbbbbb;
		width: 80%;
	}
</style>
