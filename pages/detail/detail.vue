<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
			<view class="author">{{detail.author}}</view>
			<view class="time">{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
			
		</view>
		<view class="description">
			免责声明:本站出现的所有内容仅供学术交流使用,禁止产生利益,违者后果自负,与我无关.
		</view>
		
	</view>
</template>

<script >
	import dayjs from "dayjs"
	export default {
		data() {
			return {
				options:null,
				detail:{}
			};
		},
		onLoad(e) {
			// 形参e就是从父组件newsbox传过来的数据，即每个文章的classname、id两个键值对，利用这俩键值对向后端发请求，就能准确获取这篇新闻内容的详细信息
			this.options=e;
				console.log(e);
			this.getDetail()
		},
		onShow() {
			this.getDetail()
		},
		methods:{
			getDetail(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:this.options, 
					success:res=>{
							console.log(res.data);
						res.data.posttime=dayjs(res.data.posttime).format('YYYY-MM-DD HH:mm:ss')
						// gi是正则表达式
						console.log(res.data.content);
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
						this.detail=res.data,
						// 每当用户浏览一个新闻后，执行savehistory方法，将该新闻保存到本地存储localstorage里面
						this.saveHistory(),
						// 改变页面最顶部的标题
						uni.setNavigationBarTitle({
							title:this.detail.title
						})
					}
				})
			},
			saveHistory(){
				let historyArr=uni.getStorageSync("historyArr")||[]
			let item={
				id:this.detail.id,
				classid:this.detail.classid,
				picurl:this.detail.picurl,
				title:this.detail.title,
				looktime:dayjs(Date.now())
			}
			let index=historyArr.findIndex(i=>{
				return i.id==this.detail.id
			})
			if(index>=0){
				historyArr.splice(index,1)
			}
			historyArr.unshift(item)
			
			historyArr=historyArr.slice(0,10);
			// 借助setStroageSync方法将保存了更多item元素的变量historyArr的值存到本地存储中属性名为historyArr的属性值里面，会覆盖原先本地存储中属性名为historyArr的值
			uni.setStorageSync("historyArr",historyArr)
		   }
		}
	
	}
</script>

<style lang="scss">
  .detail{
		padding: 30rpx;
		.title{
			font-size: 46rpx;
			color:#333;
		}
		// <!-- 文章编辑,时间的信息样式 -->
		.info{
			background:#f6f6f6;
			padding:20rpx;
			margin: 20rpx 0;
			font-size:25rpx;
			color:#666;
			display: flex;
			justify-content: space-between;
		}
		.content{
			padding-bottom:50rpx;
			/deep/ img{
				max-width: 100%;
			}
		}
		.description{
			background: #fef0f0;
			font-size: 26rpx;
			padding: 20rpx;
			color:#f89898;
			line-height: 1.8em;
		}
	}
</style>
