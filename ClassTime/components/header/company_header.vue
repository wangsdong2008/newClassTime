<template>
	<view class="header-title">
	    <view class="login_center login_title_txt">{{name}}<br /><span>{{en_name}}</span></view>
	</view>	
</template>

<script>
	export default {
		props:{
			msg: String
		},
		data() {
			return {
				msgarr:'',
				name:'',
				en_name:''
			}
		},
		created:function(){
			this.msgarr = this.msg;
			uni.hideTabBar({});	
			this.init();
			//console.log(this.msg);
		},	
		methods:{
			init(){
				let arr = this.msgarr.split(",");
				this.name = arr[0];
				this.en_name = arr[1];
			}
		}
	}
</script>