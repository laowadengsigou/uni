<template>
	<view class="">
		<view class="vi">
			<view class="le">
				<image src="../../static/hot.png" mode="" class="img"></image>
			</view>
			<view class="le">
				超英热门
			</view>
		</view>
		<scroll-view scroll-x>
			<view class="dv">
				<view v-for="(item,index) in list" class="left">
					<view @click="jupe(item,index)">
						<view>
							<image :src="item.cover" mode="" class="ima"></image>
						</view>
						<view class="name">
							{{item.name}}
						</view>
						<view class="box">
							<view class="">
								<uni-rate disabled="true" size="18" :value="(item.score/2)" max="5"></uni-rate>
							</view>
							<view class="text">
								{{item.score}}
							</view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import uniRate from "../../components/uni-rate/uni-rate.vue"
	export default {
		components: {
			uniRate
		},
		props: {},
		data() {
			return {
				list: [],
			}
		},
		methods: {
			re() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=122212489&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						this.list = res.data.data
						// console.log(this.list);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			jupe(item,index){
				uni.navigateTo({
					url:`../details/details?item=${JSON.stringify(item)}`
				})
			}
		},
		mounted() {
			this.re()
		}
	}
</script>

<style>
	.vi {
		display: flex;
		margin-top: 5%;
	}

	.img {
		width: 40rpx;
		height: 40rpx;
	}

	.le {
		margin-left: 3%;
	}

	.ima {
		width: 200rpx;
		height: 200rpx;
	}

	.dv {
		display: flex;
	}

	.left {
		margin-left: 2%;
		margin-top: 2%;
	}

	.name {
		font-size: 30rpx;
		width: 200rpx;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}

	.box {
		display: flex;
	}

	.text {
		font-size: 25rpx;
	}
</style>
