<template>
	<view class="content">
		<!-- <input class="uni-input" v-model="user" placeholder="请输入姓名" /> -->

		<input class="uni-input" password type="text" v-model="token" placeholder="请输入密钥" />

		<button class="but" type="primary" @click="login()">验证身份</button>
	</view>
</template>

<script>
	import App from "../../App.vue"
	export default {
		data() {
			return {
				user: "",
				token: "",
				myToken: "114514"
			}
		},
		onLoad() {
			var that = this
		},
		methods: {
			login: function() {
				var that = this
				console.log(that.token)
				if (that.token.length != 0) {
					if (that.token == that.myToken) {
						//App.saveUser(that.user)
						uni.showToast({
							title: "验证成功"
						})
						setTimeout(function() {
							uni.navigateTo({
								url: 'index'
							})
						}, 1000)
					} else {
						uni.showToast({
							title: "密钥不正确",
							icon: "none"
						})
					}
				} else {
					uni.showToast({
						title: "请输入密钥",
						icon: "none"
					})
				}
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
		border-radius: 70upx;
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
		margin-top: 200upx;
		background: #f2f2f2;
		margin-bottom: 0;
	}

	.uni-input {
		margin: auto;
		width: 80%;
		height: 10%;
		margin-top: 100upx;
		background: #f2f2f2;
		margin-bottom: 0;
		border-radius: 70upx;
	}
</style>
