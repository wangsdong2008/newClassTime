<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">我的消息</view>	
				<view class="icenter">					
					<view class="main-body write lists">
						<view class="titles ll">{{message_title}}</view>
						<view class="contents ll" >{{message_content}}</view>
						<view class="times ll">{{message_time}}</view>
					</view>	
					
					<view class="footer">
						<footerNav :msg="footer"></footerNav>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import headerNav from "@/components/header/users_header.vue"
	import footerNav from "@/components/footer/footer_nav.vue"
	
	var _self;
	
	export default {
	    components: {			
			headerNav,footerNav
		},		
		onLoad(options){
			_self = this;
			this.checkLogin(0);
			_self.message_id = options['id'];
		},
		onReady() {
			this.show();
		},
		data(){
			return{
				message_id:0,
				message_title:'',
				message_content:'',
				message_time:'',
				headermsg:'会员中心,Member Center',
				footer:'',
			}
		},
		methods:{			
			show(){
				let ret = this.getUserInfo();
				const data = {
				    guid: ret.guid,
				    token: ret.token,
					id:_self.message_id
				};
				this.getData(data);
			},			
			getData(data){
				this.sendRequest({
				    url : _self.MessageshowUrl,
				    method : _self.Method,
				    data : {"token":data.token,"guid":data.guid,"id":data.id,"t":Math.random()},
				    hideLoading : true,
				    success:function (res) {
						if(res){
							let data = res.messagelist;
							if(res.status == 3){
								_self.message_title = data.message_title;
								_self.message_content = data.message_content;
								_self.message_time = data.addtime;
							}
						}
				    }
				},"1","");
			}		
		}
	}
		
</script>

<style>	
	.titles{
		border-bottom: 1upx solid #ccc;
		padding-bottom: 20upx;
	}
	.contents{
		padding-top: 20upx;
		line-height: 45upx;
	}
	.times{
		text-align: right;
		margin-right: 20upx;
	}
	
	.ctitles{
		background:url(../../../static/img/message.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}	
	.icenter{
		width: 95%;
		margin: 0 auto;
	}
	.contents{
		padding: 40upx 0upx 0upx 0upx;
		font-size: 30upx;
		line-height: 50upx;
	}
	.times{
		padding: 20upx 0upx 40upx 0upx;		
		font-size: 30upx;
		text-align: left;
	}
	
	.titles{
		padding-bottom: 20upx;
		border-bottom:1px solid #ccc;
		font-size: 35upx;
	}
	


	
</style>
