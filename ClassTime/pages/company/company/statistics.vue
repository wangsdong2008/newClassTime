<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">全部统计</view>			
			<uni-collapse>
			    <uni-collapse-item title="上课统计" thumb="../../../static/img/stj.png">
			        <view style="padding: 30rpx;">
			           <uni-list>
						   <uni-list-item title="单人查询" @tap="bindtj(1)" :show-arrow="true" /></uli-list-item>
						   <uni-list-item title="整体查询" @tap="bindcompanytj(1)"  :show-arrow="true" /></uli-list-item>
					   </uni-list>
			        </view>
			    </uni-collapse-item>
			</uni-collapse>
			
			<uni-collapse>
			    <uni-collapse-item title="吃饭统计" thumb="../../../static/img/ftj.png">
			        <view style="padding: 30rpx;">
			           <uni-list>
						   <uni-list-item title="单人查询"  @tap="bindtj(2)" :show-arrow="true" /></uli-list-item>
						   <uni-list-item title="整体查询" @tap="bindcompanytj(2)" :show-arrow="true" /></uli-list-item>
					   </uni-list>
			        </view>
			    </uni-collapse-item>
			</uni-collapse>
			
			<uni-collapse>
			    <uni-collapse-item title="员工统计" thumb="../../../static/img/etj.png">
			        <view style="padding: 30rpx;">
			           <uni-list>
						   <uni-list-item title="单人查询"  @tap="bindtj(3)" :show-arrow="true" /></uli-list-item>
						   <uni-list-item title="整体查询" @tap="bindcompanytj(3)" :show-arrow="true" /></uli-list-item>
					   </uni-list>
			        </view>
			    </uni-collapse-item>
			</uni-collapse>
			
			<uni-collapse>
			    <uni-collapse-item title="体温查询" thumb="../../../static/img/tw.png">
			        <view style="padding: 30rpx;">
			           <uni-list>
						   <uni-list-item title="单人查询"  @tap="bindtw" :show-arrow="true" /></uli-list-item>
						   <!-- <uni-list-item title="整体查询" @tap="" :show-arrow="true" /></uli-list-item> -->
					   </uni-list>
			        </view>
			    </uni-collapse-item>
			</uni-collapse>
			
		</view>
			
		</view>
	</view>
</template>

<script>
	import service from '@/service.js';
	import mInput from '@/components/m-input.vue'
	import headerNav from "@/components/header/company_header.vue"
	import uniList from "@/components/uni-list/uni-list.vue"
	import uniListItem from "@/components/uni-list-item/uni-list-item.vue"
	import uniSection from '@/components/uni-section/uni-section.vue'
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'
	var _self;
	
	import {
	    mapState,
	    mapMutations
	} from 'vuex'
	
	export default {
	    components: {
			headerNav,uniList,uniListItem,uniSection,uniCollapse,uniCollapseItem
		},
		onLoad:function() {
			_self = this;
			_self.checkLogin(2);			
		},
		data(){
			return{
				headermsg:'统计,Statistics',
				dataList:[]
			}
		},
		methods:{
			bindtw(){
				_self.navigateTo('statisticstw');
			},
			bindtj(id){
				if(id == 3){
					_self.navigateTo('statisticsmember?id='+id);	
				}
				else{
					_self.navigateTo('statisticsstudents?id='+id);	
				}
				
			},
			bindcompanytj(id){
				_self.navigateTo('statisticscompany?id='+id);	
			},
			change(e) {}
			
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
		padding:0upx;
		width:94%;
		margin: 0 auto;
	}
</style>
