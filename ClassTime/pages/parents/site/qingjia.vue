<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">请假管理</view>
				
				<view class="register_account_input clear">
					<view class="uni-list-cell-left fz30">选择孩子：</view>
					<view class="searchinput input-txt">
						<picker focus @change="ChildPickerChange($event)" :value="child_index" :range="child_dataList">
							<view class="uni-input fz30">{{child_dataList[child_index]}}</view>
						</picker>
					</view>
				</view>
				
				<view class="register_account_input clear">
					<view class="uni-list-cell-left fz30">选择日期：</view>
					<view class="searchinput input-txt">
						<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
							<view class="uni-input fz30">{{date}}</view>
						</picker>
					</view>
				</view>
				
				<view class="register_account_input clear">
					<view class="uni-list-cell-left fz30">选择课程：</view>
					<view class="searchinput input-txt">
						<picker @change="CoursePickerChange($event)" :value="course_index" :range="course_dataList">
							<view class="uni-input fz30">{{course_dataList[course_index]}}</view>
						</picker>
					</view>
				</view>
				
				<view class="register_account_input clear">
					<view class="uni-list-cell-left fz30">请假原因：</view>
					<view class="searchinput input-txt">
						<m-input class="m-textarea" type="text" clearable focus v-model="reason" placeholder="请假原因"></m-input>
					</view>
				</view>
				
				
				<view class="btn-row">
					<button type="primary" class="primary btn" @tap="bindmodify">{{btntxt}}</button>
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
	import mInput from '../../../components/m-input.vue';
	import headerNav from "@/components/header/company_header.vue"
	import footerNav from "@/components/footer/footer_nav.vue"
	var _self;
	export default {
	    components: {
			service,
			headerNav,footerNav,
			mInput
		},
		computed: {
			startDate() {
				return _self.getDate('start');
			},
			endDate() {
				return _self.getDate('end');
			}
		},
		onLoad(){
			_self = this;
			_self.checkLogin(1);
			_self.headermsg = "请假,Ask for leave";
		},
		onReady(){
			_self.show();
		},
		data(){
			return{
				child_id:0,
				child_index:0,
				child_dataList:[],
				child_dataIDList:[],
				
				date:'==请选择日期==',
				date1:'==请选择日期==',
				time:'==请选择时间==',
				dateid:'',
				dateid1:'',
				
				course_id:0,
				course_index:0,
				course_dataList:[],
				course_dataIDList:[],
				
				reason:'',
				
				btntxt:"请假",
				footer: ''
			}
		},
		methods:{
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			bindDateChange: function(e) {
				_self.date = e.target.value
				_self.dateid = e.target.value;
				_self.getchildcourselist();
			},
			bindDate1Change: function(e) {
				_self.date1 = e.target.value
				_self.dateid1 = e.target.value;
			},
			getchildcourselist:function(){ //根据child_id和日期，获取当天的课程，用来调课			
				if(_self.child_id > 0 && _self.dateid != ''){
					let ret = _self.getUserInfo();
					if(!ret){
						return false;
					}else{
						_self.sendRequest({
							url : _self.GetDayChildCourseUrl,
							method : _self.Method,
							data : {
								"token":ret.token,
								"guid":ret.guid,
								"dateid":_self.dateid,
								"id":_self.child_id,
								"t":Math.random()
							},
							hideLoading : true,
							success:function (res) { //获取某个孩子这一天的课程
								//debugger;
								//console.log("获取数据:" + JSON.stringify(res));
								/* let data1 = res.data;
								_self.fillData(data1); */		
								var courselist = res.courselist;
								let list = [];
								let idlist = [];
								list.push("==请选择课程==");
								idlist.push(0);
								for (var i = 0; i < courselist.length; i++) {
									var item = courselist[i];
									list.push(item.c_name+"("+item.p_time+")");
									idlist.push(item.c_id);
								}								
								_self.course_dataList = list;
								_self.course_dataIDList = idlist;
								
								
							}
						},"1","");
					}
					
				}
			},
			bindTimeChange: function(e) {
				_self.time = e.target.value;
			},
			ChildPickerChange: function(e) {
			    console.log('picker发送选择改变，携带值为', e.target.value+"===="+_self.child_dataList[e.target.value] + _self.child_dataIDList[e.target.value]);
				_self.child_id = _self.child_dataIDList[e.target.value];
				_self.child_index = e.target.value;
				_self.getchildcourselist();
			},
			CoursePickerChange:function(e){
				console.log('picker发送选择改变，携带值为', e.target.value+"===="+_self.course_dataList[e.target.value] + _self.course_dataIDList[e.target.value]);
				_self.course_id = _self.course_dataIDList[e.target.value];
				_self.course_index = e.target.value;
			},
			bindmodify(){
				if(_self.child_id == 0 || _self.child_id == undefined){
					uni.showToast({
					    icon: 'none',
					    title: '请选择孩子'
					});
					return;
				}
				if(_self.dateid == '' || _self.dateid == undefined){
					uni.showToast({
					    icon: 'none',
					    title: '请选择调课日期'
					});
					return;
				}
				if(_self.course_id == 0 || _self.course_id == undefined){
					uni.showToast({
						icon: 'none',
						title: '请选择课程'
					});
					return;					
				}
				
				
				let ret = _self.getUserInfo();
				if(!ret){
					return false;
				}
				
				_self.sendRequest({
				       url : _self.updateChildCourseQingjiaSignUrl,
				       method : _self.Method,
				       data : {
							"guid": ret.guid,
							"token": ret.token,
							"id": _self.child_id,
							"dateid":_self.dateid,
							"cid":_self.course_id,
							"dateid1":_self.dateid1,
							"time":_self.time,
							"reason":_self.reason,
							"t":Math.random()
					   },
				       hideLoading : false,
				       success:function (res) {
						//debugger;
						let status = res.status;
						status = parseInt(status);
						let str = '';
						switch(status){
							case 0:{
								str = '数据填写错误';
								break;
							}
							case 1:{
								str = '日期选择错误';
								break;
							}
							case 2:{
								str = '已经请假了';
								break;
							}
							case 3:{
								str = '请假成功';
								break;
							}							
						}
						
						uni.showModal({
						    title: str,
						    content: '请选择返回的页面',
							confirmText:'返回前页',
						    success: function (res) {
						        if (res.confirm) {
									_self.navigateTo('index');
						        } 
						    }
						});
				       }
				   },"1","");			
			},
			show(){
				//if(_self.child_id == 0 ) return;  //考虑添加功能,允许等于0
				let ret = _self.getUserInfo();
				if(!ret){
					return false;
				}
				const data = {
				    guid: ret.guid,
				    token: ret.token,
					id:_self.child_id
				};
				_self.getData(data);
				
			},
			getData(data){
				this.sendRequest({
				       url : this.AllChildrenUrl,
				       method : _self.Method,
				       data : {
						"guid": data.guid,
						"token":data.token,
						"t":Math.random()
					   },
				       hideLoading : true,
				       success:function (res) {
						if(res){
							//所有孩子
							var childlist = res.list;
							var list = [];
							var idlist = [];
							list.push("==请选择孩子==");
							idlist.push(0);
							for (var i = 0; i < childlist.length; i++) {
								var item = childlist[i];
								list.push(item.child_name);
								idlist.push(item.child_id);
							}								
							_self.child_dataList = list;
							_self.child_dataIDList = idlist;
										
							var list = [];
							var idlist = [];
							list.push("==请选择课程==");
							idlist.push(0);												
							_self.course_dataList = list;
							_self.course_dataIDList = idlist;	
										
							_self.reason = "";
							
						}
				    }
			    },"1","");
			}
		}
    }
</script>

<style>	
	.m-textarea{
		border:1px solid #ccc;				
		border-radius: 50upx;
		width:400upx;
		height: 75upx;
		line-height: 75upx;
		padding-left: 40upx;
	}
	.ctitles{
		background:url(../../../static/img/qingjia.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}
	
	.pickerlist{
			width: 98%;
		}
		.listq{
			padding: 20upx 0upx;
			margin-top: 20upx;
		}
		
		.texts{
			margin-right: 10upx;
			float: left;
			width: 20%;
		}
	.left_txt{
			width:30%;
			float: left;
		}	
		.week-list{
			padding-left: 20upx;
			border:1px solid #ccc;
			border-radius: 25upx;		
			margin-top: 20upx;
		}			
		
	
		
		.register_account_input view{
			float: left;
		}
		
	
		.register_account_input{
			padding-top: 20upx;
			padding-bottom: 10px;
			line-height: 60upx;
			height: 60upx;
		}
		
	
		
		
		.uni-list-cell-left{
			margin-right: 40upx;
			width:25%;
		}
		
		picker view{
			border: 1px solid #ccc;
			width:450upx;
			text-align: center;
			height: 75upx;
			line-height: 75upx;
			border-radius: 50upx;
		}
	
</style> 