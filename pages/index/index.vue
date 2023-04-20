<template>
	<view class="home">
		<scroll-view scroll-x="true" class="nav-scroll">
			<view class="item" :class="index == navIndex ? 'active' : ''" v-for="(item, index) in navArr"
				@click="clickNav(index, item.id)" :key="item.id">{{ item.classname }}</view>
		</scroll-view>

		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<NewsBox :item="item" @click.native="goToDetail(item)"></NewsBox>
			</view>
		</view>

		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
			<text>暂无数据</text>
		</view>
		
		<view class="loading" v-if="newsArr.length">
			<view v-if="loading === 1">数据加载中...</view>
			<view v-if="loading === 2">没有更多了~~</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1,
				loading: 0 // 0 默认   // 1 加载中	// 2 没有更多了
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		onReachBottom() {
			console.log('到底啦！')
			if(this.loading == 2) return
			this.currentPage++
			this.loading = 1
			this.getNewsData()
		},
		methods: {
			// 导航切换
			clickNav(index, id) {
				this.navIndex = index
				// console.log(id)
				this.currentPage = 1
				this.newsArr = []
				this.loading = 0
				this.getNewsData(id)
			},

			// 跳转到详情页
			goToDetail(item) {
				uni.navigateTo({
					url: `/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},

			// 获取导航列表数据
			getNavData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						console.log(res)
						this.navArr = res.data
					}
				})
			},

			// 获取新闻列表数据
			getNewsData(id = 50) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: id,
						page: this.currentPage
					},
					success: res => {
						console.log(res)
						if(res.data.length === 0) this.loading = 2
						this.newsArr = [...this.newsArr, ...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.nav-scroll {
		position: fixed;
		left: 0;
		top: var(--window-top);
		z-index: 99;
		height: 100rpx;
		background: #F7F8FA;
		white-space: nowrap;

		/deep/::-webkit-scrollbar {
			display: none;
		}

		.item {
			font-size: 40rpx;
			line-height: 100rpx;
			display: inline-block;
			padding: 0 30rpx;
			color: #333;

			&.active {
				color: #31C27C;
			}
		}
	}

	.content {
		padding: 20rpx;
		padding-top: 130rpx;

		.row {
			border-bottom: 1px dashed #efefef;
			padding: 20rpx 0;
		}
	}
	
	.nodata {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image {
			width: 360rpx;
		}
		text {
			color: #888;
		}
	}

	.loading {
		font-size: 26rpx;
		text-align: center;
		color: #888;
		line-height: 2em;
		padding: 20rpx 0;
	}
</style>