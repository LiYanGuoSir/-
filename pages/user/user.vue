<template>
	<view class="user">
		<!-- 个人中心展示页 -->
		<view class="top">
			<image src="../../static/images/user.jpg" style="height: 200rpx; width: 200rpx;"></image>
			<view class="text">浏览历史</view>


		</view>


		<view class="content">
			<div class="row" v-for="item in listArr">
				<!-- newsbox组件里面配置项prop接收了父组件传过去的名字为item(该属性的属性值的类型在newsbox的配置项props里面已经被限制成了对象)的键值对 ，在此就是将listarr数组中的元素即对象传给该组件-->
				<newsbox :item="item"></newsbox>
			</div>
		</view> 
		
		<view class="nohistory" v-if="!listArr.length">
			<view class="text">暂时没有浏览记录</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]

			};
		},
		// onload只是在该页面加载完后执行一次就不执行了。相当于onload只会执行一次，哪怕再次点击个人中心跳转到这个页面也不会让onload再次执行。onshow则是每当该页面展示的时候都会执行一次。从而实现了浏览历史动态更新的需求
		onShow() {
			this.getData()
		},
		methods: {
			// 个人中心中浏览历史中的数据是来自于本地缓存中的historyArr属性的属性值，historyArr属性的属性值是在新闻详情页组件即detail组件里定义属性值的
               getData(){
				   let hisArr=uni.getStorageSync("historyArr")||[];
				   this.listArr=hisArr;	   	   
			   }
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #f8f8f8;
			color: #666;
			display: flex;
			flex-direction: column;
			// justify-content: center;
			align-items: center;

			.text {
				font-size: 38rpx;
				padding-top: 20rpx;

			}
		}

		.content {
			padding: 30rpx;

			.row {
				/* 底部点虚线dotted */
				border-bottom: 1px dotted #efefef;
				padding: 20rpx 0;
			}
		}
		.nohistory{
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			.text{
				font-size: 26rpx;
				color: #888;
			}
		}
	}
</style>