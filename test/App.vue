<script>
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		},
		saveUser: function(user) {
			try {
				uni.setStorageSync('user', user)
				console.log(user)
				console.log('存储user完成')
			} catch (e) {
				console.log(e)
			}
		},
		getUser: function() {
			try {
				var user = uni.getStorageSync('user')
				console.log('获取user完成')
				console.log(user)
				return user
			} catch (e) {
				console.log(e)
			}
		},
	}
</script>

<style>
	/*每个页面公共css */
	html,body,#app{
	 
	 height: 100%;
	 
	}
</style>
