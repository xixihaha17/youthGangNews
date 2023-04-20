<template>
	<view class="news-box">
		<view class="item">
			<view class="img">
				<image :src="item.picurl" mode="aspectFit"></image>
			</view>

			<view class="text">
				<view class="title">{{ item.title }}</view>
				<view class="info" v-if="!item.looktime">
					<text>{{ item.author }}</text>
					<text>{{ item.hits }}浏览</text>
				</view>
				<view class="info" v-else>
					<text>浏览时间：{{ item.looktime }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "NewsBox",
		props: {
			item: {
				type: Object,
				default () {
					return {
						// title: "组件内默认标题",
						// author: "饭团",
						// hits: 999,
						// picurl: "../../static/images/tuantuan.jpg",
						// looktime: "2023-04-19 09:39:20"
					}
				}
			}
		},
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss" scoped>
	.news-box {

		// display: flex;
		// justify-content: space-between;
		.item {
			display: flex;

			.img {
				width: 230rpx;
				height: 160rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.text {
				// border: 1px solid pink;
				display: flex;
				flex: 1;
				padding-left: 15rpx;
				flex-direction: column;
				justify-content: space-between;

				.title {
					font-size: 36rpx;
					color: #333;
					text-overflow: -o-ellipsis-lastline;

					// 文本设置超出2行显示省略号
					overflow: hidden; //溢出内容隐藏
					text-overflow: ellipsis; //文本溢出部分用省略号表示
					display: -webkit-box; //特别显示模式
					-webkit-line-clamp: 2; //行数
					line-clamp: 2;
					-webkit-box-orient: vertical; //盒子中内容竖直排列
				}

				.info {
					font-size: 26rpx;
					color: #999;

					text {
						margin-right: 20rpx;
					}
				}
			}
		}
	}
</style>