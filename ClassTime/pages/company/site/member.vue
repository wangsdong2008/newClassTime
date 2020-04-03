<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">全部员工</view>		
				<view class="icenter">
					<!-- 一般用法 -->
					<uni-collapse>					
						<uni-collapse-item v-for="(item,index) in dataList" :title="item.com_name" :open="true" thumb="../../../static/img/company.png" :index="index" :key="item.com_id" >
							<uni-list>
								<uni-list-item v-for="(item2,index2) in item.memberlist" :show-arrow="false" :title="item2.true_name" :index="index2" :key="item2.id" >
									<view class="statuslist fz30"><span @tap="memberedit(item2.id)">修改</span><span @tap="memberdel(item2.id)">删除</span></view>
									</uni-list-item>
							</uni-list>	
							
						</uni-collapse-item>
					   
					</uni-collapse>
				</view>
				<view class="btn-row">
					<button type="primary" class="btn" @tap="memberadd">添加员工</button>
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
	import footerNav from "@/components/footer/footer_nav.vue"
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'	
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
				headermsg:'员工管理,Member Manage',
				footer:''
			}
		},
		methods:{
			memberadd(){
				_self.navigateTo('memberedit');
			},
			memberedit(id){				
				_self.navigateTo('memberedit?id='+id);
			},
			memberdel(id){
				let ret = uni.getStorageSync(_self.USERS_KEY);
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
				_self.sendRequest({
				        url : _self.DelMemberinfoUrl,
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
				        					title: '删除员工成功',
				        					icon: 'none',
				        				});	
				        			}
				        			else{
				        				uni.showToast({
				        					title: '删除失败',
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
				        url : this.GetAllMemberUrl,
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
				       			
				       	    }
				       	}
				       
				    },"1","");
			}
		}
	}
	
</script>

<style>	
	.ctitles{
		background:url(../../../static/img/userHL.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}	
	.icenter{
		width: 95%;
		margin: 0 auto;
	}	
	.subcategory{
		background:url(../../../static/img/school.png) 50upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
		padding-left: 80upx;
	}
	.statuslist{
		position:absolute;
		right: 30upx;
		margin-top: 0upx;
	}
	.statuslist span{
		margin-right: 10upx;
	}	
	
	
</style>
