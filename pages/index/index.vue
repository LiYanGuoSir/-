<template>
	<!-- 新闻网首页最上方的滑动导航栏 -->
	<view class="home">
		<!-- uniapp封装好的滑动导航条标签<scroll-view>,scroll-x=true代表允许横向滑动 -->
		<scroll-view scroll-x>
			<view class="navscroll">				
				<view class="item" :class="index==navIndex? 'active' : '' " v-for="(item,index) in navArr"
					:key="item.id" @click="clickNav(index,item.id)">{{item.classname}}</view>
			</view>
		</scroll-view>



		<!-- 新闻网导航栏下方的内容展示区 -->
		<view class="content">
			<div class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item"></newsbox>
			</div>
		</view>

		<!-- 添加内容到底了的提示，文章列表数据有newsArr长度就不显示该提示 -->
		<view class="nodata" v-if="!newsArr.length">
			<div>内容到底了，去其他地方看看吧</div>
		</view>
		<!-- 需求1，当展示新闻列表被用户下拉到底时，还未获取到更多新闻内容时，出现“正在加载中”的提示，即显示该view标签-->
		<view class="loading" v-if="newsArr.length">
			<view v-if="loading==1">数据加载中...</view>
			<view v-if="loading==2">没有更多了...</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: "1",
				// 存储从后端获取到的首页导航栏上的字的数据
				navArr: [],
				// 存储从后端获取到的新闻列表数据
				newsArr: [],
				// 存储向后端发送的请求中的参数currentPage的值。用于控制后端发送过来的第几页数据
				currentPage: 1,
				// 控制提示“正在加载中”、“没有更多了”的显示，默认值0代表什么提示都不显示
				loading:0  
			}
		},
		// 小程序的生命周期，该方法在页面加载时触发
		onLoad() {
			this.getNavData();
			this.getNewData()
		},
		// 需求2.难点十、十一。十二合计才实现用户下拉加载更多内容并且不影响其他导航栏展示的不同新闻列表的功能。
		onReachBottom() {
			this.loading=1;
			this.currentPage++;
			this.getNewData()
		},
		methods: {
			clickNav(index, id) {
				this.navIndex = index;
				this.newsArr = [];
				this.loading=0;
				this.currentPage = 1;
				this.getNewData(id)
			},
			getNavData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						this.navArr = res.data;
						// console.log(res.data)
					}
				})
			},
			// 获取新闻列表数据
			getNewData(id = 5) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					// data属性的值同样会发送给后端
					data: {
						num: 8,
						cid: id,
						page: this.currentPage
					},
					success: res => {
						if(res.data.length==0){
							this.loading=2;
						}
						// 难点十一，用户下拉到底部后获取新的新闻数据res.data时，要把旧的新闻列表数据（即当前this.newsArr的值）跟新获得的新闻列表数据拼接起来。...是es6新添加的扩展运算符，...【】相当于把数组中所有的元素都写出来，【】创建新数组的意思
						this.newsArr = [...this.newsArr, ...res.data];
						// console.log(res.data)
					}
				})
			}

		}
	}
</script>

// scope 范围领域,代表该style标签中的样式只适用于该组件中的template中的html代码
<style lang="scss">
	/* 直接给scroll-view标签加上flex布局是不会生效的，可以在该标签里面套一个view，给这个view开启flex布局 */
	.navscroll {
		/* 	height:250rpx; */
		background-color: #f7f8fa;
		display: flex;
		position: fixed;
		top: 0;
		.item {
			color: #333;
			white-space: nowrap;
			font-size: 40rpx;
			padding: 0 30rpx;
			line-height: 100rpx;
			&.active {
				color: #31c27c;
			}
		}
	}

	.content {
		padding: 30rpx;
		padding-top: 100rpx;

		.row {
			/* 底部点虚线dotted */
			border-bottom: 1px dotted #efefef;
			padding: 20rpx 0;
		}
	}
	.loading{
		// 设置文本居中对齐
		text-align: center;
		fint-size:26rpx;
		color: #888;
		// 行高为字体大小的两倍
		line-height: 2em;
	}
</style>