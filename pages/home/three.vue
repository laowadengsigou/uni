<template>
	<view class="">
		<view class="list" v-for="(item,index) in lists">
			<view class="ima">
				<image :src="item.cover" mode="" class="img"></image>
			</view>
			<view class="bor">
				<view class="">
					{{item.name}}
				</view>
				<view class="">
					<uni-rate disabled="true" size="18" :value="(item.score/2)" max="5"></uni-rate>
				</view>
				<view class="text">
					{{item.basicInfo}}
				</view>
				<view class="text">
					{{item.releaseDate.slice(0,15)}}
				</view>
			</view>
			<view class="te">
				<view class="">
					<image src="../../static/icos/praise.png" mode="" class="pra"></image>
				</view>
				<view class="co">
					赞一下
				</view>
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
		props:{
			lists:{
				type:Array
			}
		},
		data() {
			return {

			}
		},
		methods: {

		},
		mounted() {
		}
	}
</script>

<style>
	.img{
		margin-left: 2%	;
		width:90%;
		height: 300rpx;
	}
	.list{
		display: flex;
		margin-top: 5%;
	}
	.ima{
		width:200rpx;
	}
	.text{
		font-size: 30rpx;
		width: 350rpx;
	}
	.bor{
		border-right:1rpx dashed #C0C0C0 ;
		width: 350rpx;
	}
	.pra{
		width: 50rpx;
		height: 50rpx;
	}
	.te{
		text-align: center;
		margin-left: 5%;
		margin-top: 10%;
		}
		.co{
			color: #FFCC00;
			}
</style>
