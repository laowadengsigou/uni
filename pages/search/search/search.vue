<template>
	<view>
		<view class="">
			<view class="">
				<mSearch @search="search" :placeholder="placeholder"></mSearch>
			</view>
			<view class="fl">
				<view class="ma" v-for="(item,index) in list">
					<view class="img">
						<image :src="item.cover" mode="" class="img1"></image>
					</view>
					<view class="name">
						{{item.name}}
					</view>
				</view>
			</view>
		</view>
		<!-- <view class="" v-for="(item,index) in list">
			{{item.name}}
		</view> -->
	</view>
</template>

<script>
	import mSearch from '@/components/mehaotian-search/mehaotian-search.vue';
	export default {
		components: {
			mSearch
		},
		data() {
			return {
				placeholder: "请输入电影名称",
				list:[]
			}
		},
		methods: {
			search(val) {
				uni.request({
					url: `${this.$api}/search/list?keywords=${val}&page=1&pageSize=12&qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.list = res.data.data.rows
						// console.log(res);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			sou() {
				uni.request({
					url: `${this.$api}/search/list?keywords=${""}&page=1&pageSize=12&qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.list = res.data.data.rows
						// console.log(this.list);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		mounted() {
			this.sou()
		}
	}
</script>

<style>
	.fl {
		display: flex;
		flex-flow: wrap;
	}

	.name {
		width: 210rpx;
	}

	.img {
		width: 210rpx;
	}

	.img1 {
		width: 100%;
		height: 280rpx;
	}

	.ma {
		margin-top: 3%;
		margin-left: 5%;
	}
</style>
