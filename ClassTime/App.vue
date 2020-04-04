<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	/* 头部信息 */
	.header-title{
		background:url('./static/img/login_title.png') #ffffff center 0 no-repeat;
	    background-size:100% 100%;
	    padding-bottom:20%
	}
	.login_center{
		width:85%;			
		margin: 0 auto;
	}
	.login_title_txt{
	    color:#fff;
	    font-family:'微软雅黑';
	    font-size:60upx;
	    padding-top:150upx;
	}
	
	.login_title_txt span{
	    font-size: 48upx;
	}	
	
	/* 整体样式 */
	.content{
		width:90%;
		margin: 0 auto;
		
	}
	.content .title{
		border-bottom: 1px solid #66ccff;
		height: 45upx;
		line-height: 45upx;
		margin: 30upx 0upx;
		padding-bottom: 30upx;
		background-color: #fff;
	}
	
	.title image{
		width: 50upx;
		height: 50upx;
		margin-right: 20upx;
	}
	
	/* 按纽样式 */
	.btn-row{
		margin-top: 45upx;
	}
	
	.btn{
		background-color: #55c5ff;
		border-radius: 50upx;
		border: 0px;
		text-align: center;		
		color: #ffffff;
		font-size: 30upx;
		margin-bottom: 20upx;
	}
	
	.gg{
		color:#666;
	}
	
	.fz25{
		font-size: 25upx;
	}
	
	.fz30{
		font-size: 30upx;
	}
	
	.fz35{
		font-size: 35upx;
	}
	
	.fz40{
		font-size: 40upx;
	}
	
	.clear{
		clear: both;
	}
	
	/*会员中心样式*/
	.lists{
		width: 100%;
		margin: 0 auto;
		padding:0upx;
		
	}
	
	.lists ul{
		padding: 0;
		list-style-type: none;
	}
	
	/*验证码按钮*/
	.btn1{
		background-color: #55c5ff;
		color:#fff;
		border: 0upx;
		padding:0upx;
		height: 55upx;
		line-height: 55upx;
		font-size: 30upx;
	}
	
	.m-input{
		border: 0upx;
		font-size: 28upx;
		height: 55upx;
		line-height: 55upx;
	}
	
	.footer{		
		height: 100upx;
	}
	
	uni-list{
		padding: 0;
		margin: 0;
	}
	
	.uni-list-item{
		padding-left: 0;
	}
	
	/*家长设置*/
	.contents{
		background-color: #f8f8f8;
		width:100%;
	}
	.sites{
		width:95%;
	}
	.ctitles{
		padding-left: 65upx;
		line-height:45upx;
		height: 30upx;
		padding-top: 20upx;
		background-color: #fff;
	}
	
</style>
