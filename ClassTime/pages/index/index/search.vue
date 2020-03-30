<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">查询</view>
				<view class="searchlist">
				<view class="icenter">
					<view class="imgs">
						<m-input class="m-input" type="text" clearable focus v-model="course_name" placeholder="填写课程名"></m-input>
						
					</view>
					<view class="btn">
						<button type="primary" class="primary" @tap="bindsearch">搜机构</button>
					</view>
					
				</view>
			</view>
			</view>
		</view>		
		<view class="footer">
			<footerNav :msg="footer"></footerNav>
		</view>
	</view>
</template>
<script>
	import service from '../../../service.js';
	import mInput from '@/components/m-input.vue';
	import headerNav from "@/components/header/company_header.vue"
	import footerNav from "@/components/footer/footer_nav.vue"
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	var _self;
	export default {
	    components: {
			headerNav,footerNav,
			uniList,
			uniListItem,
			mInput
		},
		onLoad(){
			_self = this;
			_self.checkLogin(0);
		},
		data(){
			return{
				headermsg:'查找机构,Search School',				
				course_name:'',
				longitude:0,
				latitude:0,
				footer:''
			}
		},
		onReady(){
			this.show();
		},
		methods:{
			show(){
				//获取经纬度
				uni.getLocation({
				  // 默认为 wgs84 返回 gps 坐标，
				  // gcj02 返回国测局坐标，可用于 uni.openLocation 的坐标
				  type: 'wgs84',
				  geocode: true,
				  success: (data) => {
				    this.longitude = data.longitude;
					this.latitude = data.latitude;
				  },
				  fail: (err) => {
				    console.log(err)
				    // this.$api.msg('获取定位失败');
				  }
				});
			},
			bindsearch(){
				if(!service.checkNull(this.course_name)){
					uni.showToast({
					    icon: 'none',
					    title: '课程不能为空'
					});
					return;
				}
				this.navigateTo('searchresult?keyword='+this.course_name+"&longitude="+this.longitude+"&latitude="+this.latitude);
			}
		}
	}
	
</script>
<style>	
	.ctitles{
		background:url(../../../static/img/tjsearch.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}
	.uni-list-item{
		width:100%;
		padding:0upx;
	}
	
	.main_center{
		width:95%;			
		margin: 0 auto;
	}
	
	.imgs{	
		border:1upx solid #ccc;	
		padding-left: 20upx;
		width: 60%;		
		padding: 10upx;
		background:url(../../../static/img/search2.png) no-repeat 10upx 25upx;
		background-size: 60upx 60upx;
		float: left;
		border-radius: 15upx;
	}
	
	.m-input{
		height: 80upx;
		line-height: 80upx;
		margin-left: 60upx;
		float: left;
	}
	
	.btn{
		width:30%;
		margin-left: 20upx;
		float: left;
	}	
	
	
</style>