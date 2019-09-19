<template>
	<view>
		<view class="vi">
			<view class="le">
				<image src="../../static/icos/108x108.png" mode="" class="img"></image>
			</view>
			<view class="le">
				热门预告
			</view>
		</view>                          
		<view class="div">
			<view class="list" v-for="(item,index) in list">
					<video @play="play" :id="item.id" :poster="item.cover" :src="item.trailer" class="trailer" controls=""></video>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: []
			}
		},
		methods: {
			play(e){
				console.log(e.currentTarget.id);
				this.list.map(item=>{
					if(item.id != e.currentTarget.id){
						uni.createVideoContext(item.id).pause()
					}
				})
			},
			yu() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=122212489&type=trailer`,
					method: 'POST',
					data: {},
					success: res => {
						this.list = res.data.data
						// console.log(this.list);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		mounted() {
			this.yu()
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

	.trailer {
		width: 96%;
		height: 300rpx;
		margin-left: 2%;
	}

	.div {
		display: flex;
		flex-wrap: wrap;
	}
	.list {
		width: 50%;
	}
	
</style>
