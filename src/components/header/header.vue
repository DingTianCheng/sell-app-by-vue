<template>
	<div class="header">
		<!-- 内容主体 -->
		<div class="content-wapper">
			 <!-- 头像 -->
			 <div class="avatar">
				 <img :src="seller.avatar" alt="" width="64" height="64" />
			 </div>
			 <div class="content">
				 <div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				 </div>
				 <div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				 </div>
				 <div v-if="seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				 </div>
			 </div>
			 <div v-if="seller.supports" class="support-count" @click="showDetail()">
			 	 <span class="count">{{seller.supports.length}}个</span>
			 	 <i class="iconfont icon-arrow-right"></i>
			 </div>
		</div>
		<!-- 公告 -->
		<div class="bulletin-wapper" @click="showDetail()">
			 <span class="bulletin-title"></span>
			 <span class="bulletin-text">{{seller.bulletin}}</span>
			 <i class="iconfont icon-arrow-right"></i>
		</div>
		<!-- 背景图片 -->
		<div class="background">
			 <img :src="seller.avatar" alt="" width="100%" height="auto" />
		</div>
		<!-- 详情弹出层 -->
		<div class="detail" v-show="detailShow">
			<div class="detail-wapper clearfix">
				<div class="detail-main">
					<h1 class="detail-main-title">{{seller.name}}</h1>
				</div>
			</div>
			<div class="detail-close">
				 <i class="iconfont icon-chahao" @click="hiddenDetail()"></i>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: [ 'seller' ],
	data () {
		return {
			detailShow: false
		};
	},
	methods: {
		showDetail () {
			this.detailShow = true;
		},
		hiddenDetail () {
			this.detailShow = false;
		}
	},
	created () {
	this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	}
};
</script>

<style scoped>
	 .header{
	 	 position: relative;
		 color:#fff;
		 background: rgba(7,17,27,0.5);
		 overflow: hidden;
	 }
	/* 内容主体 */
		.content-wapper{
		 	 position: relative;
			 padding: 24px 12px 18px 24px;
			 font-size: 0;
		 }
		 .content-wapper .avatar{
			 display: inline-block;
		 }
		 .avatar img{
			 border-radius: 2px;
		 }
		 .content-wapper .content{
			 display: inline-block;
			 vertical-align: top;
			 margin-left: 16px;
		 }
		 .content .title{
			 margin:2px 0px 8px 0px;
		 }
		 .title .brand{
			 display: inline-block;
			 width: 30px;
			 height: 18px;
			 vertical-align: top;
			 background: url(brand@2x.png) 0px 0px no-repeat;
			 background-size: 30px 18px;
		 }
		 .title .name{
			 font-size: 16px;
			 font-weight: bold;
			 line-height: 18px;
			 margin-left: 6px;
		 }
		 .content .description{
		 	margin-bottom: 10px;
		 	line-height: 12px;
		 	font-size: 12px;
		 }
		 .content .supports{

		 }
		 .icon{
		 	display: inline-block;
		 	vertical-align: top;
		 	width: 12px;
		 	height: 12px;
		 }
		 .icon.decrease{
		 	background: url(decrease_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .icon.discount{
		 	background: url(discount_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .icon.guarantee{
		 	background: url(guarantee_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .icon.invoice{
		 	background: url(invoice_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .icon.special{
		 	background: url(special_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .text{
		 	margin-left: 4px;
		 	line-height: 12px;
		 	font-size: 10px;
		 }
		 .content-wapper .support-count{
		 	position: absolute;
		 	right: 12px;
		 	bottom: 14px;
		 	padding: 7px 8px;
		 	border-radius: 14px/16px;
		 	background: rgba(0,0,0,0.2);
		 }
		 .support-count .count{
		 	margin-right: 2px;
		 	line-height: 12px;
		 	font-size: 10px;
		 	vertical-align: top;
		 }
		 .support-count i{
		 	 font-size: 10px;
		 	 line-height: 12px;
		 }

	/* 公告 */
		.bulletin-wapper{
			position: relative;
			height: 28px;
			padding: 0px 22px 0px 12px;
			background: rgba(7,17,27,0.2);
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
		}
		.bulletin-title{
			display: inline-block;
			vertical-align: top;
			width: 22px;
			height: 12px;
			margin-top: 8px;
			background: url(bulletin@2x.png) 0px 0px no-repeat;
			background-size: 22px 12px;
		}
		.bulletin-text{
			line-height: 28px;
			font-size: 10px;
		}
		.bulletin-wapper i{
			font-size: 10px;
			position: absolute;
			right: 12px;
			bottom: 7px;
		}

	/* 背景 */
		.background{
			position: absolute;
			left:0px;
			top: 0px;
			width: 100%;
			height: 100%;
			z-index: -1;
			filter: blur(10px);
			overflow: hidden;
		}

	/* 详情弹出层 */
	.detail{
		position: fixed;
		left:0;
		top:0;
		z-index: 100;
		width: 100%;
		height: 100%;
		background: rgba(7,17,27,0.8);
		overflow: auto;
	}
	.detail-wapper{
		min-height: 100%;
		overflow: hidden;
	}
	.detail-wapper .detail-main{
		margin-top:64px;
		padding-bottom: 96px;
	}
	.detail-main-title{
		margin-bottom: 16px;
		line-height: 16px;
		font-size: 16px;
		font-weight: 700;
		text-align: center;
	}
	.detail-close{
		height: 32px;
		line-height: 32px;
		margin-top: -64px;
		text-align: center;
	}
	.detail-close i{
		color: #fff;
		font-size: 32px;
	}

	@media(min-device-pixel-ratio:1.5),(-webkit-min-device-pixel-ratio:1.5){
			 .title .brand{
				 background: url(brand@3x.png) 0px 0px no-repeat;
				 background-size: 30px 18px;
			 }
			 .icon.decrease{
			 	background: url(decrease_1@3x.png) 0px 0px no-repeat;
			 	background-size: 12px 12px;
			 }
			 .icon.discount{
			 	background: url(discount_1@3x.png) 0px 0px no-repeat;
			 	background-size: 12px 12px;
			 }
			 .icon.guarantee{
			 	background: url(guarantee_1@3x.png) 0px 0px no-repeat;
			 	background-size: 12px 12px;
			 }
			 .icon.invoice{
			 	background: url(invoice_1@3x.png) 0px 0px no-repeat;
			 	background-size: 12px 12px;
			 }
			 .icon.special{
			 	background: url(special_1@3x.png) 0px 0px no-repeat;
			 	background-size: 12px 12px;
			 }
			 .bulletin-title{
			 	background: url(bulletin@3x.png) 0px 0px no-repeat;
				background-size: 22px 12px;
			 }
		}
</style>