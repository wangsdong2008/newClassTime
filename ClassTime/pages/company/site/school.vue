<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">全部学校</view>	
				<view class="icenter">
					<!-- 一般用法 -->
					<uni-collapse>					
						<uni-collapse-item v-for="(item,index) in dataList" :title="item.com_name" :open="true" thumb="../../../static/img/company.png" :index="index" :key="item.com_id" >
							<uni-list>
								<uni-list-item v-for="(item2,index2) in item.schoollist" :show-arrow="false" :title="item2.school_name" :index="index2" :key="item2.school_id" >
								<view class="statuslist fz30"><span @tap="schooledit(item2.school_id)">修改</span><span @tap="schooldel(item2.school_id)">删除</span></view>
								</uni-list-item>
							</uni-list>
						</uni-collapse-item>
					</uni-collapse>
				</view>
				<view class="btn-row">
					<button type="primary" class="btn" @tap="schooladd">添加学校</button>
				</view>
			</view>			
		</view>
		<view class="footer">
				<footerNav :msg="footer"></footerNav>
			</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	import headerNav from "@/components/header/company_header.vue"
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'	
	import footerNav from "@/components/footer/footer_nav.vue"
	var _self;
	export default {
	    components: {
			uniList,
			uniListItem,
			headerNav,footerNav,
			uniCollapse,uniCollapseItem
		},
		onLoad(){	
			_self = this;
			_self.checkLogin(2);
		},
		onReady(){
			_self.show();
		},
		data(){
			return{
				dataList:[],				
				headermsg:'学校管理,School Manage',
				footer:''
			}
		},
		methods:{
			schooladd(){
				_self.navigateTo('schooledit');
			},
			schooledit(id){				
				_self.navigateTo('schooledit?id='+id);
			},
			schooldel(id){
				let ret = _self.getUserInfo();
				if(!ret){
					return false;
				}
				const data = {
				    guid: ret.guid,
				    token: ret.token,
					id:id
				};
				_self.delData(data);
			},
			delData(data){
				this.sendRequest({
				        url : this.DelSchoolInfoUrl,
				        method : _self.Method,
				        data : {
							"guid": data.guid,
							"token":data.token,
							"id":data.id,
							"t":Math.random()
						},
				        hideLoading : false,
				        success: (res) => {
				        	    if(res){
									if(parseInt(res.status) == 3){
										_self.show();
										uni.showToast({
											title: '删除学校成功',
											icon: 'none',
										});	
									}
									else{
										uni.showToast({
											title: '删除失败，请检查此学校是否还有学生',
											icon: 'none',
										});	
									}
				        	    }
				        	},
				        
				    },"1","");							
			},
			show(){
				let ret = _self.getUserInfo();
				if(!ret){
					return false;
				}
				const data = {
				    guid: ret.guid,
				    token: ret.token
				};
				_self.getData(data);
			},
			getData(data){
				this.sendRequest({
				        url : this.GetAllSchoolUrl,
				        method : _self.Method,
				        data : {
							"guid": data.guid,
							"token":data.token,
							"t":Math.random()
						},
				        hideLoading : true,
				        success: (res) => {
				       	    if(res){
				       	    	var data = res.subcompanylist; 
				       			if(parseInt(res.status) == 3){
				       				if(data.length > 0){
				      					let list = [];
				      					for (var i = 0; i < data.length; i++) {
				      						var item = data[i];
				      						list.push(item);
				      					}								
				      					_self.dataList = list;
				      				}
				       			}
				       			else{
				       				uni.showToast({
				       					title: '无数据为空',
				       					icon: 'none',
				       				});		
				       				
				       			}
				       	    }
				       	}
				       
				    },"1","");
			}
		}
	}
	
</script>

<style>
	.ctitles{
		background:url(../../../static/img/school.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}	
	.icenter{
		width: 95%;
		margin: 0 auto;
	}
	
	.statuslist{
		position:absolute;
		right: 30upx;
		margin-top: 10upx;
	}
	.statuslist span{
		margin-right: 10upx;
	}
	
</style>
