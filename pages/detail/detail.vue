<template>
	<view class="detail">
		<view class="title">
			{{ detail.title }}
		</view>
		
		<view class="info">
			<view class="author">编辑：{{ detail.author }}</view>
			<view class="time">发布时间：{{ detail.posttime }}</view>
		</view>
		
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		
	</view>
</template>

<script>
	import { parseTime } from "@/utils/tool.js"
	
	export default {
		data() {
			return {
				options: null,
				detail: {}
			};
		},
		onLoad(e) {
			// console.log(e)
			this.options = e
			this.getDetail()
		},
		methods: {
			getDetail() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/detail.php",
					data: this.options,
					success: res => {
						console.log(res)
						res.data.content = res.data.content.replace(/<img/gi, '<img style="max-width:100%"')
						res.data.posttime = parseTime(res.data.posttime)
						this.detail = res.data
						
						this.saveHistory()
						
						uni.setNavigationBarTitle({
							title: this.detail.title
						})
					}
				})
			},
			saveHistory() {
				let historyArr = uni.getStorageSync("historyArr") || []
				let item = {
					id: this.detail.id,
					classid: this.detail.classid,
					title: this.detail.title,
					picurl: this.detail.picurl,
					looktime: parseTime(Date.now())
				}
				
				let index = historyArr.findIndex(i => {
					return i.id == this.detail.id
				})
				if(index >= 0) {
					historyArr.splice(index, 1)
				}
				
				historyArr.unshift(item)
				historyArr = historyArr.slice(0, 6)
				uni.setStorageSync("historyArr", historyArr)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.detail {
		padding: 20rpx;

		.title {
			font-size: 36rpx;
		}

		.info {
			background: #F6F6F6;
			padding: 20rpx 0;
			display: flex;
			justify-content: space-between;
			color: #666;
			font-size: 25rpx;
			margin: 30rpx 0;
		}

		.content {
			// img {
			// 	max-width: 100%;
			// }
		}

	}
</style>