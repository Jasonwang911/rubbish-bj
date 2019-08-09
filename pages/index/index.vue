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
			<text class="value" @click="handleTimePicker">{{}}</text>
			<text class="eosfont icon">&#xe616;</text>
		</view>
		<w-picker :mode="mode" startYear="2016" endYear="2030" step="1" :current="true" ref="picker" themeColor="#f00" :selectList="selectList"></w-picker>
	</view>
</template>

<script>
	import amap from '../../common/utils/amap-wx.js';  
	import wPicker from "@/components/w-picker/w-picker.vue";
	
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
				}  
			}
		},
		components: {
			wPicker
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
			// 掉起时间picker
			handleTimePicker() {
				this.$refs.picker.show();
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
</style>
