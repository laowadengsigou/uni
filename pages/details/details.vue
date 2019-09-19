<template>
	<view class="cor">
		<view class="" v-if="flag === false">
			<view class="">
				<video :poster="list.cover" :src="(list.trailer)" class="trailer"></video>
			</view>
			<view class="top">
				<view class="">
					<image :src="list.cover" mode="" class="img" @click=click(list.cover)></image>
				</view>
				<view class="te">
					<view class="">
						{{list.name}}
					</view>
					<view class="text">
						{{list.basicInfo}}
					</view>
					<view class="text">
						{{list.originalName}}
					</view>
					<view class="text">
						{{list.releaseDate}}
					</view>
					<view class="fl">
						<view class="">
							<view class="">综合评分</view>
							<view class="shu">{{list.score}}</view>
						</view>
						<view class="left">
							<view class="">
								<uni-rate disabled="true" size="18" :value="(list.score/2)" max="5"></uni-rate>
							</view>
							<view class="text">
								{{list.prisedCounts}}人点赞
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="ju">
				<view class="text1">
					剧情介绍
				</view>
				<view class="plotDesc">
					{{list.plotDesc}}
				</view>
			</view>
			<view class="">
				<view class=""></view>
			</view>
			<view class="ju">
				<view class="text1">
					演职人员
				</view>
				<scroll-view scroll-x>
					<view class="one">
						<view style="margin-top: 3%;">
							<view class="">
								<image :src="photo" mode="" class="photo" @click="click(photo)"></image>
							</view>
							<view class="name">
								{{name}}
							</view>
							<view class="actName">
								{{actName}}
							</view>
						</view>
						<view style="margin-top: 3%;margin-left: 3%;" v-for="(item,index) in trailerIds">
							<view class="">
								<image :src="item.photo" mode="" class="photo"></image>
							</view>
							<view class="name">
								{{item.name}}
							</view>
							<view class="actName">
								饰演{{item.actName}}
							</view>
						</view>
					</view>
				</scroll-view>
			</view>
			<view class="jus">
				<view class="text1">剧照</view>
				<view class="fl">
					<view class="img1" v-for="(item,index) in JSON.parse(list.plotPics)">
						<image :src="item" mode="" class="ige" @click="click(item)"></image>
					</view>
				</view>
			</view>
		</view>
		<view class="re" v-else>
			<view class="ab" @click="clicks">

			</view>
			<image :src="cover" mode="" class="image" @longpress="longpress" @click="clicks"></image>
			<view class="abc" @click="clicks">

			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from "../../components/uni-rate/uni-rate.vue"
	export default {
		components: {
			uniRate
		},
		data() {
			return {
				list: {},
				photo: "", //导演图片
				actName: "", //导演
				name: "", //导演名字
				trailerIds: [], //演员
				image: [], //剧照
				cover: "",
				flag: false
			}
		},
		methods: {
			click(cover) {
				this.cover = cover
				console.log(cover);
				this.flag = true
			},
			clicks() {
				this.flag = false
			},
			role() {
				uni.request({
					url: `${this.$api}/search/staff/${this.list.id}/1?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.photo = res.data.data[0].photo
						this.name = res.data.data[0].name
						this.actName = res.data.data[0].actName
						// console.log(res.data.data[0]);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			trailerId() {
				uni.request({
					url: `${this.$api}/search/staff/${this.list.id}/2?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.trailerIds = res.data.data
						// console.log(this.trailerIds);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			longpress() {
				let _this = this
				uni.showActionSheet({
					itemList: ['保存图片到本地'],
					success() {
						uni.downloadFile({
							url: _this.cover,
							success(res) {
								console.log(res);
								if (res.statusCode === 200) {
									uni.saveImageToPhotosAlbum({
										filePath: res.tempFilePath,
										success() {
											uni.showToast({
												title: '保存成功'
											})
										},
										fail() {
											uni.showToast({
												title: "保存失败"
											})
										}
									})
								}
							}
						})
					}
				})
			}
		},
		mounted() {
			this.role()
			this.trailerId()
		},
		onLoad(e) {
			this.list = JSON.parse(e.item)
			// console.log(this.list);
		}
	}
</script>

<style>
	.re {

	}

	.ab {
		width: 100%;
		height: 400rpx;
		background-color: #000000;
	}

	.image {
		width: 100%;

	}

	.abc {
		width: 100%;
		height: 400rpx;
		background-color: #000000;
		margin-top: -12rpx;
	}

	.shu {
		color: #FFCC00;
	}

	.fl {
		display: flex;
		flex-flow: wrap;
	}

	.img1 {
		width: 220rpx;
		margin-left: 2%;
	}

	.ige {
		width: 100%;
		height: 320rpx;
	}

	.one {
		display: flex;
	}

	.actName {
		font-size: 30rpx;
		color: #C8C7CC;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
		width: 150rpx;
	}

	.name {
		width: 150rpx;
		font-size: 30rpx;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}

	.photo {
		width: 150rpx;
		height: 200rpx;
	}

	.trailer {
		width: 100%;
	}

	.top {
		width: 96%;
		margin-left: 2%;
		margin-top: 10%;
		display: flex;
		border-bottom: 5rpx solid #999999;
		padding-bottom: 150rpx;
	}

	.img {
		width: 250rpx;
		height: 350rpx;
		margin-left: 5%;
	}

	.text {
		font-size: 30rpx;
		color: #C0C0C0;
	}

	.te {
		margin-left: 5%;
	}

	.cor {
		background-color: #F8F8F8;
	}

	.fl {
		display: flex;
		background-color: #FFFFFF;
		margin-top: 13%;
		height: 100rpx;
	}

	.left {
		margin-left: 10%;
		margin-top: 5%;
	}

	.text1 {
		font-size: 30rpx;
		color: #C0C0C0;
	}

	.ju {
		width: 96%;
		margin-left: 2%;
		margin-top: 4%;
		border-bottom: 5rpx solid #999999;
		padding-bottom: 50rpx;
	}

	.jus {
		width: 96%;
		margin-left: 2%;
		margin-top: 4%;
	}

	.plotDesc {
		margin-top: 4%;
		font-size: 35rpx;
	}
</style>
