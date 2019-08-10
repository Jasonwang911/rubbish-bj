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
	/*每个页面公共css */
@import './common/css/uni.css';
@import './common/css/eosfont.css';

uni-page-body, page {
	background-color: #E6E6E6;
	font-size: 30upx;
	width: 100%;
}

</style>
