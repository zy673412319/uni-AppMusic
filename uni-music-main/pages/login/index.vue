<template>
  <div class="login-container">
		<view class="login-main">
			<image src="../../static/img/logo.jpg" mode="widthFix" class="login-logoImg"></image>
			<view class="login-Input flexDiv">
				<view class="iconfont icon-phone"></view>
				<view class="loinput">
					<input placeholder="请输入手机号码" v-model="phone">
				</view>
			</view>
			<view class="login-Input flexDiv">
				<view class="iconfont icon-mima"></view>
				<view class="loinput">
					<input placeholder="请输入密码" v-model="password">
				</view>
			</view>
			<button class="cu-btn block margin-tb-sm lg" @click="getUserInfo">
				登录
			</button>
		</view>
  </div>
</template>

<script>
import { login } from '@/api/modules/user.js'
import { getCache } from "@/utils/cache.js"
export default {
  data() {
    return {
      loading: false,
			phone: '',
			password: '',
    }
  },
	mounted(){
	},
  methods: {
    getUserInfo() {
			console.log("ssxscsc");
      if (this.loading) return;
      let that = this
      const userInfo = {
        phone: this.phone,
        password: this.password
      }
      this.loading = true
			login(userInfo).then((res) => {
				this.$store.dispatch('login', {
					account: res.account,
					cookie: res.cookie
				});
				this.loading = false
				uni.switchTab({
					url: '/pages/index/index',
				})
				// res.cookie
			}).catch((err) => {
				this.loading = false;
				this.showToastFun(err.message);
			})
    },
		showToastFun(test){
			uni.showToast({
				title: test || '登录失败',
				icon: 'none',
				duration: 3000,
				complete: function () {
					setTimeout(function () {
						uni.hideToast()
					}, 3000)
				},
			})
		},
		
  },
}
</script>

<style lang="scss" scoped>
.login-container {
  width: 100%;
  height: 100%;
	text-align: center;

	.login-main {
		width: 90%;
		margin: 0 auto;
		
		
		.login-logoImg {
			width: 320rpx;
			margin: 20px auto;
		}
		
		.login-Input {
			width: 100%;
			height: 50px;
			border: 1px solid #ededed;
			border-radius: 4px;
			margin-bottom: 20px;
			.iconfont {
				font-size: 20px;
				width: 40px;
			}
			.loinput {
				width: calc(100% - 50px);
				height: auto;
				line-height: 1.5;
				text-align: left;
			}
		}
	}
	
  .cu-btn {
		background: #dd2525;
		color: white;
		margin-top: 80px;
	}
}
</style>
