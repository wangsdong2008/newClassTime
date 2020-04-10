<template>
	<view class="login_content">
		<headerNav :msg="headermsg"></headerNav>
		<view class="contents">
			<view class="content sites">
				<view class="title ctitles fz35">注册新用户</view>	
				<view class="icenter">
					<view class="register_account_input">				
						<m-input class="m-input register-input register-input-mobile" type="text" clearable focus v-model="mobile" placeholder="填写手机号码"></m-input>
					</view>
					<view class="register_account_input fz30">
						<m-input class="m-input register-input register-input-mail" type="text" clearable v-model="code" placeholder="填写验证码"></m-input>
						<button class="btn fz30 btn1" @tap="send_sms">获取验证码</button>
					</view>
					<view class="clear"></view>
					<view class="register_account_input fz30">
						<m-input class="m-input register-input register-input-password" displayable type="password" clearable v-model="password" placeholder="6-20位登录密码"></m-input>
					</view>
					<view class="register_account_input fz30">
						<m-input class="m-input register-input register-input-password" displayable type="password" clearable v-model="againpassword" placeholder="再次确认密码"></m-input>
					</view>
					<view class="register_account_input fz30">
						<m-input class="m-input register-input register-input-mobile" type="text" clearable focus v-model="recommend" placeholder="推荐人手机号码"></m-input>
					</view>
					<view class="btn-row">
						<button class="btn" type="primary" @tap="register">注册新用户</button>
					</view>	
				</view>
			</view>
		</view>		
	</view>
</template>

<style>	
	.ctitles{
		background:url(../../../static/img/register.png) 10upx 25upx no-repeat;
		-webkit-background-size: 40upx 40upx;
		background-size: 40upx 40upx;
	}	
	.icenter{
		width: 95%;
		margin: 0 auto;
	}
	
     .register_account{
		margin-top: 30upx;
		margin-bottom: 20upx;
	}
	
	.register_account_input{
		padding-top: 20upx;
		padding-bottom: 10px;
		border-bottom: 1px solid #eeeeee;		
	}	

	.register-input{		
		width:90%;
		height: 70upx;
		padding-left: 90upx;
	}
	.register-input-username{
		background:url(../../../static/img/user.png) no-repeat;
		-webkit-background-size:30upx 42upx ;
		background-size:30upx 42upx ;
	}
	.register-input-mobile{
		background:url(../../../static/img/mobile.png) no-repeat;
		-webkit-background-size:50upx 62upx ;
		background-size:50upx 62upx ;
	}
	.register-input-mail{
		background:url(../../../static/img/mail.png) no-repeat;	
		width:48%;
		float: left;
	}	
	
	.register-input-password{
		background:url(../../../static/img/password.png) no-repeat;		
	}
</style>

<script>
    import service from '../../../service.js';
    import mInput from '../../../components/m-input.vue';
	import headerNav from "@/components/header/company_header.vue";
	var _self;
	
	export default {
        components: {
            mInput,headerNav
        },
		onLoad(){
			_self = this;
		},
        data() {
            return {
				headermsg:'注册,Register',
				code:'',
                password: '',
                againpassword: '',
				mobile:'',
				recommend:'', //推荐人手机
				sessionid:''
            }
        },
        methods: {
			send_sms(){		
				if(!service.checkMobile(_self.mobile)){
					uni.showToast({
					    icon: 'none',
					    title: '手机号码不合法'
					});
					return;
				}
				
				if(_self.temp_status == 1){ //测试
					try {
						uni.clearStorageSync();  
					} catch (e) {  
					// error  
					}
					debugger;
				}
									
				var ret = uni.getStorageSync(_self.Temp_KEY);
				if(ret == undefined || ret == ""){//如果不能获取的话，获取新的sessionid，防止软件直接注册
					return false;
				}else{
					//正常的页面，通过登录页面过来的
					const data = {
					    "token": ret.token,
					    "mobile": _self.mobile,
						"smsid":ret.smsid,
						"status":1
					};
					_self.sendsms2(data);
				}
			},
            register() {                
				if(!service.checkMobile(_self.mobile)){
				    uni.showToast({
				        icon: 'none',
				        title: '手机号码不合法'
				    });
				    return;
				}
				
				if(!service.checkNull(_self.code)){
				    uni.showToast({
				        icon: 'none',
				        title: '验证码不能为空'
				    });
				    return;
				}
				
                if (_self.password.length < 6) {
                    uni.showToast({
                        icon: 'none',
                        title: '密码最短为 6 个字符'
                    });
                    return;
                }
				if (_self.againpassword.length < 6) {
				    uni.showToast({
				        icon: 'none',
				        title: '密码最短为 6 个字符'
				    });
				    return;
				}
				if(_self.againpassword != _self.password){
					uni.showToast({
					    icon: 'none',
					    title: '两次密码不一致，请重新填写！'
					});
					return;
				}
				
                const data = {
                    password: _self.password,
					againpassword: _self.againpassword,
                    mobile: _self.mobile,
					recommend:_self.recommend,
					code:_self.code
                }
                _self.addUsers(data);
                /* uni.showToast({
                    title: '注册成功'
                });
                uni.navigateBack({
                    delta: 1
                }); */
            }
        }
    }
</script>

