<template>
	<view class="main_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">公告</view>
				<view class="icenter">					
					<view class="main-body write lists">
						<view class="titles ll fz35">{{message_title}}</view>
						<view class="contents ll fz30" v-html="message_content"></view>
						<view class="times ll fz30">{{message_time}}</view>
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
			_self.guid = options['id'];
		},
		onReady() {
			this.show();
		},
		data(){
			return{
				message_title:'',
				message_content:'',
				message_time:'',
				headermsg:'会员中心,Member Center',
				footer:'',
				guid:''
			}
		},
		methods:{			
			show(){
				let ret = this.getUserInfo();
				const data = {
				    guid: ret.guid,
				    token: ret.token,
					id:_self.guid
				};
				this.getData(data);
			},			
			getData(data){
				this.sendRequest({
				    url : _self.GonggaoshowUrl,
				    method : _self.Method,
				    data : {"token":data.token,"guid":data.guid,"id":data.id,"t":Math.random()},
				    hideLoading : true,
				    success:function (res) {
						if(res){
							let data = res.articlelist;
							if(res.status == 3){
								_self.message_title = data.article_title;
								_self.message_content = data.article_content;
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
		background:url(../../../static/img/gonggao.png) 10upx 25upx no-repeat;
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
		font-size: 30upx;
		margin-top: 10upx;
	}
	.statuslist span{
		margin-right: 10upx;
	}
	
	
</style>

