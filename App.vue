<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/* 每个页面公共css */
	/* 添加公共样式 */
view{
	/* 设置宽高的值为可见框的宽与高，即此时weidth与height指的是内容区、内边距、边框的总和 */
	box-sizing:border-box;
	}
</style>
