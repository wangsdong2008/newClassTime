<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">公司相关设置</view>		
				<uni-list>
					<uni-list-item title="公司设置" thumb="../../../static/img/company.png" @tap="bindCompany" />
					<uni-list-item title="课程分类" thumb="../../../static/img/category.png" @tap="bindCategory" />
					<uni-list-item title="教室管理" thumb="../../../static/img/classroom.png" @tap="bindClassRoom" />
					<uni-list-item title="学校管理(托班)" thumb="../../../static/img/school.png" @tap="bindShcool" />
					<uni-list-item title="年级管理(托班)" thumb="../../../static/img/grade.png" @tap="bindgrade" />
					<uni-list-item title="班级管理(托班)" thumb="../../../static/img/classs.png" @tap="bindclass" />
					<uni-list-item title="学生管理" thumb="../../../static/img/students.png" @tap="bindstudent" />
					<uni-list-item title="上课安排" thumb="../../../static/img/plan.png" @tap="bindcompanyplan"  />
				</uni-list>
				<uni-list>
					<uni-list-item title="员工管理" thumb="../../../static/img/staff.png" @tap="bindmember" />				 
					<uni-list-item title="权限管理" thumb="../../../static/img/power.png" />
					<uni-list-item title="帮助文档" thumb="../../../static/img/help.png" @tap="bindhelp" />
				</uni-list>
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
	
	var _self;
	
	export default {
	    components: {
			uniList,
			uniListItem,
			headerNav,
			footerNav
		},
		data(){
			return{
				dataList:[],				
				headermsg:'公司设置,Company Siteup',
				footer: 'companysite'
			}
		},
		onLoad:function() {
			_self = this;
			_self.checkLogin(2);
		},
		methods:{
			bindmember:function(){
				_self.navigateTo('member');	
			},
			bindhelp:function(){
				_self.navigateTo('help');
			},
			bindCompany:function(){ //子公司管理
				_self.navigateTo('company');
			},
			bindCategory:function(){ //分类管理
				_self.navigateTo('category');
			},
			bindClassRoom:function(){ //教室管理
				_self.navigateTo('classroom');
			},
			bindShcool:function(){ //学校管理
				_self.navigateTo('school');
			},
			
			bindgrade:function(){ //年级管理
				_self.navigateTo('grade');
			},
			bindclass:function(){ //班级管理
				_self.navigateTo('classs');
			},
			bindstudent:function(){ //学生管理
				_self.navigateTo('students');
			},
			bindcompanyplan:function(){
				_self.navigateTo('companyplan');
			}
			
		}
	}
</script>

<style>
	.ctitles{
		background:url(../../../static/img/system.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}
	.uni-list{
		margin-top: 40upx;
		background-color: #fff;
	}
</style>
