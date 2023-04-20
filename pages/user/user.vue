<template>
	<view class="user">
		<view class="img">
			<image src="../../static/images/clock.png" mode="aspectFit"></image>
			<text>浏览历史</text>
		</view>

		<view class="content">
			<view class="row" v-for="item in listArr">
				<NewsBox :item="item" @click.native="goToDetail(item)"></NewsBox>
			</view>
		</view>
		
		<view class="nohistory" v-if="!listArr.length">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<text>暂无浏览记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr: []
			};
		},
		onShow() {
			this.getData()
		},
		methods: {
			// 跳转到详情页
			goToDetail(item) {
				uni.navigateTo({
					url:  `/pages/detail/detail?cid=${item.classid}&id=${item.id}`,
				})
			},
			
			// 获取缓存浏览数据
			getData() {
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
				console.log(this.listArr)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.user {
		.img {
			background: #F7F8FA;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 150rpx;
				height: 150rpx;
				padding: 50rpx 0 0;
			}

			text {
				font-size: 30rpx;
				color: #555;
				padding: 20rpx;
			}
		}

		.content {
			padding: 20rpx;

			.row {
				border-bottom: 1px dashed #efefef;
				padding: 20rpx 0;
			}
		}
		
		.nohistory {
			display: flex;
			flex-direction: column;
			justify-content: center;
			text {
				text-align: center;
				color: #888;
				font-size: 30rpx;
			}
		}
	}
</style>