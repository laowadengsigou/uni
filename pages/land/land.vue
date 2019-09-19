<template>
	<view>
		<view class="imgs">
			<image src="../../static/icos/default-face.png" mode="" class="img"></image>
		</view>
		<view class="box">
			<view class="name">账号</view>
			<view class="int">
				<input type="text" value="" placeholder="请输入用户名" v-model="username" />
			</view>
		</view>
		<view class="box1">
			<view class="name">密码</view>
			<view class="int">
				<input type="password" value="" placeholder="请输入密码" v-model="password" />
			</view>
		</view>
		<QSWavesButton btnStyle="margin: 30px;" @click="clickFc">注册/登陆</QSWavesButton>
		<view class="wa">
			<view class="lefts">
				<image @click="san(weixin)" src="../../static/icos/weixin.png" mode="" class="three"></image>
			</view>
			<view class="lefts">
				<image @click="san(qq)" src="../../static/icos/QQ.png" mode="" class="three"></image>
			</view>
			<view class="lefts">
				<image @click="san(sinaweibo)" src="../../static/icos/weibo.png" mode="" class="three"></image>
			</view>
		</view>
	</view>
</template>

<script>
	import QSWavesButton from "../../components/QS-WavesButton/QS-WavesButton.vue"
	export default {
		components: {
			QSWavesButton
		},
		data() {
			return {
				username: "",
				password: "",
				weixin: "weixin",
				qq: "qq",
				sinaweibo: "sinaweibo",
				infoRes: {}
			}
		},
		methods: {
			clickFc() {
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=122212489`,
					method: 'POST',
					data: {
						username: this.username,
						password: this.password,
						userBO: {}
					},
					success: res => {
						console.log(res);
						if (res.data.status === 200) {
							this.$store.state.user = res.data.data
							uni.setStorageSync("user", JSON.stringify(res.data.data))
							uni.showToast({
								title: '登陆成功'
							})
							uni.switchTab({
								url: "../my/my"
							})
						} else if (res.data.status === 500) {
							uni.showToast({
								title: res.data.msg
							})
						}
					},
					fail: () => {},
					complete: () => {}
				});
			},
			san(name) {
				console.log(name);
				uni.login({
					provider: name,
					success: ((loginRes) => {
						console.log(loginRes.authResult);
						// 获取用户信息
						let face = ''
						let nickname = ''
						let openIdOrUid = ''
						uni.getUserInfo({
							provider: name,
							success: ((infoRes) => {
								this.infoRes = infoRes
								console.log(infoRes);
								if (name === 'weixin') {
									face = infoRes.userInfo.avatarUrl
									nickname = infoRes.userInfo.nickName
									openIdOrUid = infoRes.userInfo.openId
								}
								if (name === 'qq') {
									face = infoRes.userInfo.avatarUrl
									nickname = infoRes.userInfo.nickName
									openIdOrUid = infoRes.userInfo.openId
								}
								if (name === 'sinaweibo') {
									face = infoRes.userInfo.avatarUrl
									nickname = infoRes.userInfo.nickName
									openIdOrUid = infoRes.userInfo.id
								}
								uni.request({
									url: `${this.$api}/appUnionLogin/${name}?qq=122212489`,
									method: 'POST',
									data: {
										face: face,
										nickname: nickname,
										openIdOrUid: openIdOrUid
									},
									success: res => {
										console.log(res);
										uni.setStorageSync("user",JSON.stringify(res.data.data))
										uni.switchTab({
											url: '../my/my'
										})
									},
									fail: () => {},
									complete: () => {}
								});

							})
						});
					})
				});
			}
		},

	}
</script>

<style>
	.lefts {
		margin-left: 15%;
	}

	.wa {
		display: flex;
	}

	.three {
		width: 100rpx;
		height: 100rpx;
	}

	.img {
		width: 250rpx;
		height: 250rpx;
		margin-top: 10%;
	}

	.imgs {
		text-align: center;
	}

	.int {
		margin-left: 20rpx;
		margin-top: 5rpx;
	}

	.box {
		display: flex;
		width: 90%;
		margin-left: 5%;
		height: 70rpx;
		border-bottom: #C0C0C0 solid 2rpx;
		margin-top: 120rpx;
	}

	.box1 {
		display: flex;
		width: 90%;
		margin-left: 5%;
		height: 70rpx;
		border-bottom: #C0C0C0 solid 2rpx;
		margin-top: 30rpx;
	}

	.name {
		margin-left: 20rpx;
	}
</style>
