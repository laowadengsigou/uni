<template>
	<view class="">
			<view class="box" v-if="!islogin">
				<view class="">
					<image src="../../static/icos/default-face.png" mode="" class="img" @click="click"></image>
				</view>
				<view class="text" @click="click">注册/登陆</view>
			</view>
		<view class="box" v-else>
			<view class="">
				<image :src="user.faceImage" mode="" class="img"></image>
			</view>
			<view class="text">
				<view class="">{{user.nickname}}</view>
				<view class="">ID:{{user.id}}</view>
			</view>
			<view class="">
				<image src="../../static/icos/settings.png" mode="" class="image" @click="modify(user)"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		components: {},
		props: {},
		data() {
			return {
				user: {},
				islogin: false
			}
		},
		methods: {
			click() {
				uni.navigateTo({
					url: "../land/land"
				})
			},
			modify(item) {
				uni.navigateTo({
					url: "../modify/modify"
				})
			}
		},
		mounted() {

		},
		created() {

		},
		filters: {},
		computed: {},
		watch: {},
		directives: {},
		onShow() {
			if (uni.getStorageSync("user")) {
				this.user = JSON.parse(uni.getStorageSync("user"))
				this.islogin = true
			} else {
				this.islogin = false
			}
		}
	}
</script>

<style>
	.box {
		height: 350rpx;
		background-color: #FFCC00;
		display: flex;
	}

	.img {
		width: 150rpx;
		height: 150rpx;
		margin-top: 150rpx;
		margin-left: 20rpx;
		border-radius: 50%;
	}

	.text {
		margin-top: 200rpx;
		margin-left: 50rpx;
	}

	.image {
		width: 50rpx;
		height: 50rpx;
		margin-top: 160rpx;
		margin-left: 20rpx;
	}
</style>
