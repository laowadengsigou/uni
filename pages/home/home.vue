<template>
	<view>
		<view>
			<swiper class="vi" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
				<swiper-item v-for="(item,index) in list">
					<image :src="item.image" class="img" />
				</swiper-item>
			</swiper>
		</view>
		<one></one>
		<two></two>
		<three :lists="lists"></three>
	</view>
</template>

<script>
	import one from "./one"
	import two from "./two"
	import three from "./three"
	export default {
		components: {
			one,
			two,
			three
		},
		props: {},
		data() {
			return {
				list: [], //轮播
				indicatorDots: true,
				autoplay: true,
				interval: 5000,
				duration: 500,
				suer: [], //超英
				lists:[]
			}
		},
		onLoad() {
			uni.request({
				url: `${this.$api}/index/carousel/list?qq=122212489`,
				method: 'POST',
				data: {},
				success: res => {
					this.list = res.data.data
					// console.log(this.list);
				},
				fail: (e) => {
					console.log(e);
				},
				complete: () => {}
			});
		},
		  onPullDownRefresh: function() {
		  uni.request({
		  	url:`${this.$api}/index/guessULike?qq=122212489`,
		  	method: 'POST',
		  	data: {},
		  	success: res => {
		  		this.lists = res.data.data
		  		// console.log(this.list);
				uni.stopPullDownRefresh()
		  	},
		  	fail: () => {},
		  	complete: () => {}
		  });
		  },
		methods: {
			like(){
				uni.request({
					url:`${this.$api}/index/guessULike?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.lists = res.data.data
						// console.log(this.list);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		mounted() {
			this.like()
		}
	}
</script>

<style>
	.vi {
		margin-left: 2%;
		width: 96%;
		height: 400rpx;
	}

	.img {
		width: 100%;
		height: 400rpx;
	}
</style>
