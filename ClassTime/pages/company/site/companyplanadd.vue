<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">计划信息</view>	
				<view class="icenter">		
					<view class="register_account_input">
						<picker @change="pickerCompanyChange($event)" :value="cindex" :range="cList">
							<view class="uni-input">{{cList[cindex]}}</view>
						</picker>
					</view>
					<view class="register_account_input">
						<picker @change="categoryPickerChange($event)" :value="category_index" :range="category_dataList">
							<view class="uni-input">{{category_dataList[category_index]}}</view>
						</picker>
					</view>
					
					<view :class="{
						'register_account_input':true,
						 'checkboxlist':(_self.com_id > 0)
						}">
						<view class="studentslist" v-if="_self.com_id == 0">==请选择学生==</view>
						<view v-if="_self.com_id > 0">
							<checkbox-group @change="checkboxChange">
								<label class="uni-list-cell uni-list-cell-pd" v-for="(item,index) in students_dataList" :index="index" :key="item.uid.toString()">
									<checkbox class="checkbox" :value="item.uid.toString()" /><text>{{item.uname}}</text>
								</label>
							</checkbox-group>					
						</view>
					</view>
					
					<view class="register_account_input">
						<view class="uni-list-cell-left fz35">
							上课时间
						</view>		
					</view>
					
					<view  class="register_account_input week-list">				
						<view class="week-list-time">
							<view class="left_txt">星期：</view>
							<view class="cell-right">
								<checkbox-group @change="weekcheckboxChange">
									<label class="uni-list-cell uni-list-cell-pd" v-for="(item,index) in week_dataList" :index="index" :key="item.weekid">
										<checkbox :value="item.weekid" :checked="item.shower" /><text>{{item.weektext}}</text>
									</label>
								</checkbox-group>
							</view>	
						</view>
						<view class="week-list-time">
							<view class="left_txt">上课时间:</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsutime_list" placeholder="所选接时间"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
										}">周{{item2.weektext}}
										</view>
										<picker mode="time" :value="item2.utime" start="00:01" end="23:59"  @change="bindTimeChange($event,item2.weekid)" :class="{
											'awidth':true,
												'hidden':!item2.shower
											}">
											<view class="uni-input">{{item2.utime}}</view>
										</picker>	
									</view>
								<view class="clear"></view>
								
							</view>
						</view>
						<view class="week-list-time address">
							<view class="left_txt">上课教室：</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsclassroom_list" placeholder="上课教室"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
										}">周{{item2.weektext}}
										</view>
										<picker @change="bindClassRoomChange($event,item2.weekid)" :value="item2.classroom_index" :range="classroom_dataList"  :class="{
											'awidth':true,
												'hidden':!item2.shower
											}">
											<view class="uni-input">{{classroom_dataList[item2.classroom_index]}}</view>
										</picker>
									</view>
								<view class="clear"></view>
							</view>
						</view>
						<view class="week-list-time address">
							<view class="left_txt">接的地址：</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsuaddress_list" placeholder="接的地址"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
										}">周{{item2.weektext}}
									</view>
									<view :class="{
										'awidth':true,
										'hidden':!item2.shower
									}">
									<input class="m-input t2" type="text" :value="item2.uaddress" placeholder="接的地址" @blur="bindaddress($event,item2.weekid)"></input>
									</view>	
								</view>
								<view class="clear"></view>
							</view>
							<view class="clear"></view>
						</view>
						<view class="week-list-time">
							<view class="left_txt">送的时间:</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsgivetime_list" placeholder="送的时间"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
										}">周{{item2.weektext}}
									</view>							
										<picker mode="time" :value="item2.givetime" start="00:01" end="23:59"  @change="bindgiveTimeChange($event,item2.weekid)" :class="{'awidth':true,'hidden':!item2.shower}">
											<view class="uni-input">{{item2.givetime}}</view>
										</picker>
								</view>
								<view class="clear"></view>	
							</view>	
						</view>
						<view class="week-list-time address">
							<view class="left_txt">送的地址：</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsgiveaddress_list" placeholder="送的地址"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
									}">周{{item2.weektext}}</view>
									<view :class="{
										'awidth':true,
										'hidden':!item2.shower
									}">
										<input class="m-input t2" type="text" :value="item2.giveaddress" placeholder="送的地址" @blur="bindgiveaddress($event,item2.weekid)"></input>
									</view>	
								</view>
								<view class="clear"></view>
							</view>	
							<view class="clear"></view>
						</view>
						<view class="week-list-time">
							<view class="left_txt">接回时间:</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsbacktime_list" placeholder="接回的时间"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
										}">周{{item2.weektext}}
									</view>
									<picker mode="time" :value="item2.backtime" start="00:01" end="23:59"  @change="bindbackTimeChange($event,item2.weekid)" :class="{'awidth':true,'hidden':!item2.shower}">
										<view class="uni-input">{{item2.backtime}}</view>
									</picker>
								</view>
								<view class="clear"></view>	
							</view>					
						</view>
						<view class="week-list-time address">
							<view class="left_txt">是否吃饭：</view>
							<view class="cell-right">
								<!-- <input class="m-input t2" type="text" :value="_self.studentsfanstatus_list" placeholder="吃饭状态"></input> -->
								<view v-for="(item2,index2) in week_dataList" :index="index2" :key="item2.weekid">
									<view :class="{
										'texts':true,
										'hidden':!item2.shower
									}">周{{item2.weektext}}</view>
									<view :class="{
										'awidth':true,
										'hidden':!item2.shower
									}">
									<radio-group @change="radiofanChange($event,item2.weekid)">
										<label class="uni-list-cell uni-list-cell-pd" v-for="(fan_item, fan_index) in fan_items" :key="fan_item.value">
										<view>
											<radio class="radios" :value="fan_item.value" :checked="parseInt(fan_item.value) === item2.fan_status" />
										</view>
										<view class="radio_text">{{fan_item.name}}</view>
										</label>
									</radio-group>	
									
									
									</view>	
								</view>
								<view class="clear"></view>
							</view>	
							<view class="clear"></view>
						</view>
						<view class="clear"></view>
					</view>	
					<view class="clear"></view>
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
<style>	
	.ctitles{
		background:url(../../../static/img/plan.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}	
	.icenter{
		width: 95%;
		margin: 0 auto;
	}	
	
	
	.texts{
		font-size: 25upx;
		margin-right: 10upx;
		/* border: 1px solid #f00; */
	}
	.hidden{
		display: none;
	}
	.week-list label{
		margin-right: 15upx;
		margin-bottom:10upx;
		font-size: 30upx;
		width: 120upx;
		display: block;
		float: left;
	}
	.week-list .left_txt{
		width:30%;
		font-size: 30upx;
		/* border:1px solid #f00; */
	}	
	.cell-right{
		width:65%;
		padding-right: 10upx;
		/* border:1px solid #ff0; */
	}	
	.week-list{
		padding-left: 20upx;
		border:1px solid #ccc;
		border-radius: 25upx;		
		margin-top: 20upx;
	}		
	picker,.studentslist{
		font-size: 30upx;
	}
	.awidth{
		margin-left: 10upx;
		text-align: left;		
		margin-bottom: 20upx;
		overflow: hidden;
		
	}
	.week-list picker{
		width: 300upx;
		height: 60upx;
		line-height: 60upx;	
		margin-left: 62upx;
		border:1px solid #eee;	
		padding-left: 5upx;
	}
	.week-list .m-input{
		padding-left: 10upx;
		margin-bottom: 20upx;
	}
	
	.week-list .week-list-time{
		padding-left: 20upx;
	}
	.week-list .week-list-time.address{
		clear: both;
	}
	.week-list .week-list-time view{
		float: left;
		margin-right: 0upx;
		margin-bottom: 20upx;
	}
	.week-list .week-list-time view view{
		margin-bottom: 0upx;
	}
	
	.week-list-time .cell-right .m-input{
		border: 0upx;
		border: 1px solid #eee;
		line-height: 70upx;
		height: 70upx;
		font-size: 28upx;
	}
	.week-list view{
		/* float: left;	 */	
	}
	.checkboxlist{
		font-size: 30upx;
		text-align: left;
		height: 400upx;
		overflow-y: auto;
		padding: 20upx;
		border: 1px solid #eee;
		margin:20upx 0;
	}
	.checkboxlist label{
		margin-right: 15upx;
		width:180upx;
		margin-bottom: 20upx;
		height: 40upx;
		line-height: 40upx;
		display: block;
	} 
	
	view.txt{
		font-size: 30upx;
	}
	.weeklist label{
		margin-right: 10upx;
		width:120upx;
		margin-bottom: 20upx;
		height: 40upx;
		line-height: 40upx;
		font-size: 30upx;
	} 	
	.checkboxlist .checkbox{
		height: 40upx;
		line-height: 40upx;
	}
	.weeklist{
		height: 110upx;
	}
	
	
	.register_account_input{
		padding-top: 20upx;
		padding-bottom: 10px;
		border-bottom: 1px solid #eeeeee;
		line-height: 60upx;	
		text-align: left;
	}
	
</style>
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
		onLoad(options){
			_self = this;
			_self.checkLogin(2);
			_self.uid = options['id'];
			if(_self.uid == undefined){
				_self.uid = 0;
			}
			if(_self.uid == 0){
				_self.btntxt = '添加'
				_self.headermsg = "添加新计划,Plan Add";
			}else{
				_self.btntxt="修改";
				_self.headermsg = "计划编辑,Plan Edit";
			}
		},
		onReady(){
			_self.show();
		},
		data(){
			return{
				footer:'',
				grade_id:0,
				grade_name:'',
				grade_order:'',
				grade_address:'',
				
				//教室相关
				classroom_id:0,
				classroomindex:0,
				classroom_dataList:[],
				classroom_dataIDList:[],
				
				
				dataList:[],	
							
				com_id:0,
				cindex:0,
				cList:[],
				cIDList:[],
				cStatuslist:[],
				
				//临时保存的信息
				studentsid_list:'', //所有学生id
				studentsweek_list:'1',//所选周几
				studentsutime_list:'15:00',//所选周几的接孩子时间
				studentsuaddress_list:'',//所选周几的接孩子地点
				studentsgivetime_list:'',//所选周几的送孩子时间
				studentsgiveaddress_list:'',//所选周几的送孩子地点
				studentsbacktime_list:'',//所选周几的送孩子时间
				studentsfanstatus_list:'0',//是否吃饭
				
				category_id:0,
				category_index:0,
				category_dataList:[],
				category_dataIDList:[],
				
				//学生列表
				uid:0,
				students_dataList:[],
								
				
				week_id:0,
				week_index:0,				
				week_dataList:[],
				week_dataIDList:[],
				
				fan_items: [
					{
						value: '1',
						name: '吃'
					},
					{
						value: '0',
						name: '不吃'
					}
				],
				
				
				
				ptime:"15:00",
				
				headermsg:'',
				btntxt:''
			}
		},
		methods:{
			bindmodify(){			
				//debugger;
				if(parseInt(_self.com_id) == 0){
					uni.showToast({
					    icon: 'none',
					    title: '请选择机构'
					});
					return;
				}
				if(parseInt(_self.category_id) == 0){
					uni.showToast({
					    icon: 'none',
					    title: '请选择课程'
					});
					return;
				}
				if(!service.checkNull(_self.studentsid_list+""))
				{
					uni.showToast({
					    icon: 'none',
					    title: '请选择学生'
					});
					return;
				}				
					    
				var arr0 = _self.studentsweek_list.split(",");
				var arr1 = _self.studentsutime_list.split(",");
				if(arr0.length != arr1.length){
					uni.showToast({
					    icon: 'none',
					    title: '请选择上课时间'
					});
					return;
				}
				//提交数据
				let ret = _self.getUserInfo();				
				_self.sendRequest({
				    url : _self.AddCompanyplanInfoUrl,
				    method : _self.Method,
				    data : {
						"token":ret.token,
						"guid":ret.guid,
						"com_id":_self.com_id,
						"cat_id":_self.category_id,
						"studentsidlist":_self.studentsid_list,
						"studentsweeklist":_self.studentsweek_list,
						"studentsutimelist":_self.studentsutime_list,
						"studentsuaddresslist":_self.studentsuaddress_list,
						"studentsgivetimelist":_self.studentsgivetime_list,
						"studentsgiveaddresslist":_self.studentsgiveaddress_list,
						"studentsbacktimelist":_self.studentsbacktime_list,
						"studentsclassroomlist":_self.studentsclassroom_list,
						"studentsfanstatuslist":_self.studentsfanstatus_list,
					},
				    hideLoading : true,
				    success:function (res) {
						//debugger;
						if(res){
							let status = res.status;
							let str = '';
							switch(status){
								case 0:{
									str = '数据填写错误';
									break;
								}
								case 1:{
									str = '请选择学生';
									break;
								}
								case 2:{
									str = '上课天数和上课次数不一致';
									break;
								}
								case 3:{
									_self.dataList = [];
												
									_self.com_id = 0;
									_self.cindex = 0;
									_self.cList = [];
									_self.cIDList =  [];
									_self.cStatuslist = [];
									
									//临时保存的信息
									_self.studentsid_list = ''; //所有学生id
									_self.studentsweek_list = '1';//所选周几
									_self.studentsutime_list = '15:00';//所选周几的接孩子时间
									_self.studentsuaddress_list = '';//所选周几的接孩子地点
									_self.studentsgivetime_list = '';//所选周几的送孩子时间
									_self.studentsgiveaddress_list = '';//所选周几的送孩子地点
									_self.studentsbacktime_list = '';//所选周几的送孩子时间
									_self.studentsclassroom_list = '';//所选教室
									
									
									_self.category_id = 0;
									_self.category_index = 0;
									_self.category_dataList = [];
									_self.category_dataIDList = [];
									
									//学生列表
									_self.uid = 0;
									_self.students_dataList = [];
													
									
									_self.week_id = 0;
									_self.week_index = 0;				
									_self.week_dataList = [];
									_self.week_dataIDList = [];									
									
									str = '添加成功';
									break;
								}							
							}
							
							uni.showModal({
								title: str,
								content: '请选择返回的页面',
								cancelText:'留在本页',
								confirmText:'返回前页',
								success: function (res) {
									if (res.confirm) {
										_self.navigateTo('companyplanedit');
									} else if (res.cancel) {
										_self.navigateTo('companyplanedit');
									}
								}
							});
						}
				    }
				},"1","");
			},
			radiofanChange:function(e,num){
				//debugger;
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].fan_status = e.target.value;
				_self.getWeekList();
			},
			bindClassRoomChange:function(e,num){
				//debugger;
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].classroom_index = e.target.value;
				_self.getWeekList();
			},
			bindbackTimeChange: function(e,num) {
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].backtime = e.target.value;
				_self.getWeekList();
			},
			bindgiveTimeChange: function(e,num) {
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].givetime = e.target.value;
				_self.getWeekList();
			},
			bindgiveaddress:function(e,num){
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].giveaddress = e.target.value;
				_self.getWeekList();
			},
			bindaddress:function(e,num){
				//debugger;
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].uaddress = e.target.value;
				_self.getWeekList();
				
			},
			bindTimeChange: function(e,num) {
				num = parseInt(num);
				if(num == 0) num = 7;
				_self.week_dataList[num-1].utime = e.target.value;
				
				/* let list = [];
				let data = _self.week_dataList;
				for(var i = 0;i<data.length;i++){
					if(data[i].shower){
						list.push(data[i].utime);
					}
				}
				_self.studentsutime_list = list.toString(); */
				_self.getWeekList();
			},
			getWeekList:function(){
				var items = _self.week_dataList;
				let list_utime = [];
				let list = [];
				let list_uaddress = [];
				let list_givetime = [];
				let list_giveaddress = [];
				let list_backtime = [];
				let list_classroom = [];
				let list_fan_status = [];
				for (var i = 0; i <  items.length; i++) {
				    let item = items[i];
					if(item.shower){
						list.push(item.weekid);
						list_utime.push(item.utime);
						list_uaddress.push(item.uaddress);
						list_givetime.push(item.givetime);
						list_giveaddress.push(item.giveaddress);
						list_backtime.push(item.backtime);
						list_classroom.push(_self.classroom_dataIDList[item.classroom_index]);
						list_fan_status.push(item.fan_status);
				    }
				}
				_self.studentsweek_list = list.toString();
				_self.studentsutime_list = list_utime.toString();
				_self.studentsuaddress_list = list_uaddress.toString();
				_self.studentsgivetime_list = list_givetime.toString();
				_self.studentsgiveaddress_list = list_giveaddress.toString();
				_self.studentsbacktime_list = list_backtime.toString();
				_self.studentsclassroom_list = list_classroom.toString();
				_self.studentsfanstatus_list = list_fan_status.toString();
				
			},
			weekcheckboxChange:function(e){
				var items = _self.week_dataList;
				var values = e.detail.value;
				for (var i = 0; i <  items.length; i++) {
				    let item = items[i];
				    if(values.includes(item.weekid)){
				        this.$set(item,'shower',true);						
				    }else{
						this.$set(item,'shower',false);
					}
				}
				_self.getWeekList();
			},
			checkboxChange: function (e) {
				var items = _self.students_dataList;
			    var values = e.detail.value;
				let list = [];
			    for (var i = 0; i <  items.length; i++) {
			        let item = items[i];
			        if(values.includes(item.uid.toString())){
						list.push(item.uid.toString());
			        }
			    }
				_self.studentsid_list = list.toString();
			},
			categoryPickerChange:function(e){
				console.log('学校picker发送选择改变，携带值为', e.target.value+"===="+_self.category_dataList[e.target.value] + _self.category_dataIDList[e.target.value]);
				var category_id = _self.category_dataIDList[e.target.value];
				_self.category_id = category_id;
				_self.category_index = e.target.value;
			},
			pickerCompanyChange:function(e){
				console.log('公司picker发送选择改变，携带值为', e.target.value+"===="+_self.cList[e.target.value] + _self.cIDList[e.target.value]);
				_self.com_id = _self.cIDList[e.target.value];
				_self.status = _self.cStatuslist[e.target.value];
				_self.cindex = e.target.value; 
				
				//获取下属分类
				let ret = _self.getUserInfo();
				this.sendRequest({
				    url : this.GetAllSubCompanyCategoryByComidUrl,
				    method : _self.Method,
				    data : {
						"guid": ret.guid,
						"token":ret.token,
						"com_id":_self.com_id,
						"status":_self.status,
						"t":Math.random()
					},
				    hideLoading : true,
				    success: (res) => {	
				    	if(res){
							if(parseInt(res.status) == 3){
								var data = res.categorylist;
								let uid = 0;
								let list = [];
								let idlist = [];
								list.push("==请选择课程==");
								idlist.push(0);
								for (var i = 0; i < data.length; i++) {
									var item = data[i];									
									list.push(item.cat_name);
									idlist.push(item.cat_id.toString());
								}
								_self.category_dataList = list;
								_self.category_dataIDList = idlist;
								_self.category_index = 0;
								
								//所有学生
								data = res.studentslist;
								list = [];
								for (var i = 0; i < data.length; i++) {
									var item = data[i];									
									list.push(item);
								}
								_self.students_dataList = list;
								
								//所有教室
								data = res.classroomlist;
								list = [];
								list.push("=请选择教室==");
								idlist = [];
								idlist.push(0);
								for (var i = 0; i < data.length; i++) {
									var item = data[i];									
									list.push(item.classroom_name);
									idlist.push(item.classroom_id);
								}
								_self.classroom_dataList = list;
								_self.classroom_dataIDList = idlist;
							}							
						}
					}
				});				
				
			},
			
			show(){	
				let ret = uni.getStorageSync(_self.USERS_KEY);
				if(!ret){
					return false;
				}
				const data = {
				    guid: ret.guid,
				    token: ret.token,
					id:_self.grade_id
				};		
						
				if(_self.uid == 0){
					_self.week_dataList = [					
						{"weektext":'一',"weekid":'1',"shower":true,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'二',"weekid":'2',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'三',"weekid":'3',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'四',"weekid":'4',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'五',"weekid":'5',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'六',"weekid":'6',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
						{"weektext":'日',"weekid":'0',"shower":false,"utime":_self.ptime,"uaddress":'','givetime':'00:00:00','giveaddress':'','backtime':'00:00:00',"classroom_index":0,"fan_status":0},
					];
				}
				else{
					_self.week_dataList = ['周一','周二','周三','周四','周五','周六','周日'];
					_self.week_dataIDList = ['1','2','3','4','5','6','0'];
					
				} 
				
				
				_self.getData(data);
			},
			getData(data){			
				this.sendRequest({
				    url : this.GetCompanyplanInfoUrl,
				    method : _self.Method,
				    data : {
						"guid": data.guid,
						"token":data.token,
						"id":data.id,
						"t":Math.random()
					},
				    hideLoading : true,
				    success: (res) => {
							//子公司
							var data = res.subcompanylist;
							let list = [];
							let idlist = [];
							let statuslist = [];
							list.push("==请选择所属机构==");
							idlist.push(0);
							statuslist.push(0)
							for (var i = 0; i < data.length; i++) {
								var item = data[i];									
								list.push(item.com_name);
								idlist.push(item.com_id);
								statuslist.push(item.wt_status);
							}								
							_self.cList = list;
							_self.cIDList = idlist;
							_self.cStatuslist = statuslist;
							if(_self.uid == 0) _self.cindex = 0;	
							
							
							list = [];
							idlist = [];
							list.push("==请选择课程==");
							idlist.push(0);
							_self.category_dataList = list;
							_self.category_dataIDList = idlist;
							if(_self.uid == 0)	_self.category_index = 0; 
						
				    	    if(res){
								
								
								/* var categorylist = res.categorylist;
								list = [];
								idlist = [];
								list.push("==请选择分类==");
								idlist.push(0);
								for (var i = 0; i < categorylist.length; i++) {
									var item = categorylist[i];
									list.push(item.category_name);
									idlist.push(item.category_id);
								}								
								_self.category_dataList = list;
								_self.category_dataIDList = idlist;
								if(_self.uid == 0)	_self.category_index = 0; */
								
								//debugger;
								
								var data = res.gradelist; 
				    			if(parseInt(res.status) == 3){
				    				/* _self.grade_name = data.grade_name;
				    				_self.grade_order = data.grade_order.toString();	 							
									
									_self.com_id = data.com_id;
									let j = _self.cIDList.findIndex(i => i == _self.com_id);
									_self.cindex = j;									
									
									_self.category_id = data.category_id;
									j = _self.category_dataIDList.findIndex(i => i == _self.category_id);
									_self.category_index = j;*/	
									
									
									
				    			}
								
				    	    }
				    	}
				    
				},"1","");				
			}
		}
    }
</script>