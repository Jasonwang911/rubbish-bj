<template>
	<view class="content">
		<view class="top-location">
			<text class="icon eosfont">&#xe60c;</text>
			<text class="location">{{addressName}}</text>
			<text class="eosfont">&#xe616;</text>
		</view>
		<view class="list-block">
			<text class="name">详细地址</text>
			<input class="uni-input value" v-model="address" placeholder="请输入详细地址" />
			<text class="eosfont icon">&#xe616;</text>
		</view>
		<view class="list-block">
			<text class="name">上门时间</text>
			<text class="value" @tap="handleTimePicker">{{sda }}</text>
			<text class="eosfont icon">&#xe616;</text>
		</view>
		<picker-view class="data-picker" v-if="visible" :indicator-style="indicatorStyle" :value="timeSelect" @change="bindChange">
		    <picker-view-column>
		        <view class="item" v-for="(item,index) in days" :key="index">{{item}}</view>
		    </picker-view-column>
		    <picker-view-column>
		        <view class="item" v-for="(item,index) in times" :key="index">{{item}}</view>
		    </picker-view-column>
		</picker-view>
	</view>
</template>

<script>
	import amap from '../../common/utils/amap-wx.js';  
	
	export default {
		data() {
			return {
				title: 'Hello',
				address: '',
				amapPlugin: null,  
        key: 'afc6e09bc51c1c6deb31a734c69e5125',
				addressName: '定位中...',  
				weather: {  
					hasData: false,  
					data: []  
				},
				// 上门日期选择
				days: ['今天', '明天', '后天'],
				times: ['10点-11点', '12点-13点', '14点-15点'],
				timeSelect: [0,0],
				/**
				 * 解决动态设置indicator-style不生效的问题
				 */
				visible: false,
				indicatorStyle: `height: ${Math.round(uni.getSystemInfoSync().screenWidth/(750/100))}px;`
			}
		},
		onLoad() {
			this.amapPlugin = new amap.AMapWX({  
					key: this.key  
			});  
			// this.getRegeo
			this.amapPlugin.getRegeo({  
					success: (data) => {  
							console.log(data)  
							this.addressName = data[0].regeocodeData.aois && data[0].regeocodeData.aois.length > 0 ? data[0].regeocodeData.aois[0].name : '暂时无法获取您的位置';  
							uni.hideLoading();  
					},
					fail: (err) => {
						console.log(err)
					}
			});  
		},
		methods: {
			handleTimePicker() {
				this.visible = true
			},
			// 时间选择
			bindChange (e) {
				console.log(e, e.detail.value)
			    const val = e.detail.value
			    // this.year = this.years[val[0]]
			    // this.month = this.months[val[1]]
			    // this.day = this.days[val[2]]
			},
			// 高德地图
			getRegeo() {  
				uni.showLoading({  
						title: '获取信息中'  
				});  
				this.amapPlugin.getRegeo({  
						success: (data) => {  
								console.log(data)  
								this.addressName = data[0].name;  
								uni.hideLoading();  
						}  
				});  
			} 
		}
	}
</script>

<style>
.content{
	position: relative;
}
.top-location{
	text-align: center;
	margin-bottom: 10upx;
	background: #fff;
	line-height: 80upx;
	vertical-align: top;
}
.top-location .icon{
	color: #459DF5;
}
.top-location .location {
	display: inline-block;
	margin: 0 20upx;
	font-size: 36upx;
}
.list-block{
	display: flex;
	background: #fff;
	line-height: 80upx;
	border-bottom: 1upx solid #C0C0C0;
}
.list-block .name{
	width: 120upx;
	padding-left: 10upx;
	color: #808080;
}
.list-block .value{
	flex: 1;
	text-align: right;
	line-height: 80upx;
	height: 80upx;
}
	
picker-view {
		width: 100%;
		height: 500upx;
		margin-top:20upx;
		position: absolute;
		left: 0;
		bottom: 0;
}

.item {
		line-height: 100upx;
		text-align: center;
}
</style>
