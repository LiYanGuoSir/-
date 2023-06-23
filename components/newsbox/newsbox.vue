<template>
	<view class="newbox" @click="goDetail()">
		<!-- 首页新闻展示页 -->
		<view class="pic">
		<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		
		
		<view class="text">
			<view class="title">{{item.title}}</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}</text>
			</view>
			<view class="title" v-else>{{item.looktime}}</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		data() {
			return {
				
			};
		},
		methods:{
			
			goDetail(){
				// console.log(this.item)
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${this.item.classid}&id=${this.item.id}`
				})
			}
		},
		props:{
			item:{
				type:Object,
				default(){
					return{
						title:"新闻默认标题",
						author:"LiYanGuoSir",
						hits:758,
						picurl:"../../static/images/01.jpg",
						looktime:"2023-6-12 20:53:15"
					}
				}
			}
		}
	}
</script>

<style lang="scss">
.newbox{
		 display: flex;
		 justify-content: space-between;
	
 .pic{
	 width: 230rpx;
	 height: 160rpx;
     // flex-grow: 1;
	 image{
		 width: 100%;
		 height: 100%;
	    }
	 }
	.text{
		// 图片右边的区域大小设置成去掉图片大小后的百分比宽度，否则过长的标题会挤压图片的大小。不用担心标题文字部分区域设置的百分比宽度过小，给它加个父元素有空白空间时等比例扩大即可。标题长度太长一直挤压图片，原来没有给标题title限制宽度，不然它会一直挤压其他兄弟元素的大小
		width:20%;
		padding-left: 20rpx;
		// border: 1px solid red;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		flex-grow: 1;
		.title{
			width:100%;
			font-size: 36rpx;
			color:#333;
			overflow: hidden;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-line-clamp: 2;
			// line-clamp: 2;
			-webkit-box-orient:vertical;
			
		}
		.info{
			font-size: 26rpx;
			color:#999;
			text{
				padding-right:30rpx;
			}
		}
	}
 
 }
</style>