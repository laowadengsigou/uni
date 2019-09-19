<template>
	<view>
		<view class="box">
			<view class="name">头像</view>
			<view style="display: flex;">
				<view>
					<image :src="user.faceImage" mode="" class="img" @click="img"></image>
				</view>
				<view class="">
					<image src="../../static/icos/left-gray-arrow.png" mode="" class="img1"></image>
				</view>
			</view>

		</view>
		<view class="box">
			<view class="name">昵称</view>
			<view style="display: flex;">
				<view class="nickname">
					{{user.nickname}}
				</view>
				<view class="">
					<image src="../../static/icos/left-gray-arrow.png" mode="" class="img1"></image>
				</view>
			</view>

		</view>
		<view class="box">
			<view class="name">生日</view>
			<view style="display: flex;">
				<view class="nickname">
					{{user.birthday}}
				</view>
				<view class="">
					<image src="../../static/icos/left-gray-arrow.png" mode="" class="img1"></image>
				</view>
			</view>
		</view>
		<view class="box">
			<view class="name">性别</view>
			<view style="display: flex;">
				<view class="nickname">
					{{user.sex}}
				</view>
				<view class="">
					<image src="../../static/icos/left-gray-arrow.png" mode="" class="img1"></image>
				</view>
			</view>
		</view>
		<view class="ab">
			<view class="">
				<button type="default" @click="clear">清理缓存</button>
			</view>
			<view class="">
				<button type="default" @click="out">退出登陆</button>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				user: [],
				sex: "",
				id: ""
			}
		},
		methods: {
			clear () {
				uni.clearStorageSync()
			},
			img() {
				uni.showActionSheet({
					itemList: [
						"查看我的头像",
						"从手机相册中选中",
					],
					success: res => {
						console.log(res);
						if (res.tapIndex = 1) {
							uni.chooseImage({
								count: 1, //默认9
								sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
								sourceType: ['album'], //从相册选择
								success: (res) => {
									let user = JSON.parse(uni.getStorageSync('user'))
									let file = res.tempFilePaths[0]
									console.log(file);
									uni.uploadFile({
										url: `${this.$api}/user/uploadFace?userId=${user.id}&qq=122212489`,
										filePath: file,
										name: 'file',
										header: {
											"headerUserId": user.id,
											"headerUserToken": user.userUniqueToken,
										},
										success: res => {
											uni.setStorageSync("user",res.data)
											// this.user = uni.getStorageSync('user')
										},
										fail: err => {
											console.log(err);
										}
									})
								}
							});
						}
					}
				})
			},
			out() {
				uni.request({
					url: `${this.$api}/user/logout?qq=122212489`,
					method: 'POST',
					data: {
						userId: this.user.id
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: res => {
						if (res.data.status === 200) {
							uni.showToast({
								title: "退出登陆成功"
							})
							this.$store.state.user = {}
							uni.removeStorageSync("user")
							uni.switchTab({
								url: "../my/my"
							})
						}
						console.log(res);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onShow() {
			if (uni.getStorageSync("user")) {
				this.user = JSON.parse(uni.getStorageSync("user"))
				console.log(this.user);
				// this.id = this.user.id
				// console.log(this.id);
			}
			if (this.user.sex === 1) {
				this.user.sex = "男"
			} else {
				this.user.sex = "女"
			}
		}
	}
</script>

<style>
	.box {
		display: flex;
		justify-content: space-between;
		margin-top: 20rpx;
	}

	.img {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
	}

	.img1 {
		width: 50rpx;
		height: 50rpx;
		margin-top: 10rpx;
	}

	.name {
		margin-top: 10rpx;
		margin-left: 20rpx;
	}

	.nickname {
		margin-top: 10rpx;

		color: #C0C0C0;
	}

	.ab {
		position: absolute;
		bottom: 0;
		width: 100%;
	}
</style>
