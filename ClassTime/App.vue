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
		width:96%;
		margin: 0 auto;
		
	}
	.content .title{
		border-bottom: 1px solid #66ccff;
		height: 45upx;
		line-height: 45upx;
		margin: 30upx 0upx;
		padding-bottom: 30upx;
	}
	.content .title image{
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
	}
	
	.fz30{
		font-size: 30upx;
	}
	
	.clear{
		clear: both;
	}
	
	
</style>
