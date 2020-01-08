<template>
	<view class="content">
		<camera device-position="front" flash="off" @error="error" style="width: 100%; height: 100%;">
			<cover-view style="height: 100%;width: 100%;display: flex;align-items: center;">
				<cover-view class="green" @click="yqz()"></cover-view>
			</cover-view>
		</camera>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tempThumbPath: [],
				tempVideoPath: [],
				result: {},
				src: "",
				alpha: "",
				b: "",
				c: "",
				disabled: false,
				ctx: {}
			}
		},
		onLoad() {
			var that = this
			that.ctx = uni.createCameraContext()
		},
		methods: {
			yqz: function() {
				var that = this
				uni.showToast({
					title: "录制开始",
					icon: "none"
				})
				// that.ctx.startRecord({
				// 	success: (res) => {
				// 		console.log(res)
				// 		setTimeout(function() {
				// 			that.ctx.stopRecord({
				// 				success: (res) => {
				// 					console.log(res)
				// 					that.tempThumbPath.push(res.tempThumbPath)
				// 					that.tempVideoPath.push(res.tempVideoPath)

				// 					that.time = 0
				// 					var pages = getCurrentPages();
				// 					if (pages.length > 1) {
				// 						//上一个页面实例对象
				// 						var prePage = pages[pages.length - 2];
				// 						//关键在这里
				// 						prePage.$vm.set_tempVideoPath(JSON.stringify(that.tempVideoPath))
				// 						setTimeout(function() {
				// 							uni.navigateBack({
				// 								delta: 1
				// 							})
				// 						}, 2000)
				// 					}
				// 				},
				// 				fail: (e) => {
				// 					console.log(e)
				// 				}
				// 			})
				// 		}, 5000)
				// 	}
				// })
				that.ctx.startRecord({
					success: (res) => {
						console.log(res)
						for (let count = 0; count < 5; count++) {
							(function(num) {
								setTimeout(function() {
									console.log(num)
									if (num == 0) {
										uni.showToast({
											title: "请点头",
											icon: "none"
										})
										setTimeout(function() {
											uni.showToast({
												title: "再点头",
												icon: "none"
											})
										}, 5000)
									} else if (num == 1) {
										uni.showToast({
											title: "请张嘴",
											icon: "none"
										})
										setTimeout(function() {
											uni.showToast({
												title: "再张嘴",
												icon: "none"
											})
										}, 5000)
									} else if (num == 2) {
										uni.showToast({
											title: "请一直眨眼",
											icon: "none"
										})
										setTimeout(function() {
											uni.showToast({
												title: "再一直眨眼",
												icon: "none"
											})
										}, 5000)
									} else if (num == 3) {
										uni.showToast({
											title: "请摇头",
											icon: "none"
										})
										setTimeout(function() {
											uni.showToast({
												title: "再摇头",
												icon: "none"
											})
										}, 5000)
									} else if (num == 4) {
										uni.showToast({
											title: "请保持不动",
											icon: "none"
										})
										setTimeout(function() {
											uni.showToast({
												title: "请保持不动",
												icon: "none"
											})
										}, 5000)
									}
									setTimeout(function() {
										that.ctx.stopRecord({
											success: (res) => {
												console.log(res)
												that.tempThumbPath[num] = res.tempThumbPath
												that.tempVideoPath[num] = res.tempVideoPath
												that.time = 0
												if (num < 4) {
													that.ctx.startRecord({
														success: (res) => {
															console.log(res)
														}
													})
												} else {
													uni.showToast({
														title: "录制结束",
														icon: "none"
													})
													var pages = getCurrentPages();
													if (pages.length > 1) {
														//上一个页面实例对象
														var prePage = pages[pages.length - 2];
														//关键在这里
														prePage.$vm.set_tempVideoPath(JSON.stringify(that.tempVideoPath))
														setTimeout(function() {
															uni.navigateBack({
																delta: 1
															})
														}, 2000)
													}
												}
											},
											fail: (e) => {
												console.log(e)
											}
										})
									}, 11000)
								}, num * 13000)
							})(count)
						}
					}
				})
			},
			error(e) {
				console.log(e.detail)
			},
		}
	}
</script>

<style>
	.green {
		margin: auto;
		height: 200upx;
		width: 150upx;
		border: 3upx solid #00ff00;
	}

	.notice {
		color: #ff0000;
	}

	.content {
		text-align: center;
		height: 100%;
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
		width: 100upx;
		background: #FF0000;
	}
</style>
