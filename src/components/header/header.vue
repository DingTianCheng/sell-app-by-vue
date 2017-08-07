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
		<div class="detail" v-show="detailShow" transition="fade">
			<div class="detail-wapper clearfix">
				<div class="detail-main">
					<h1 class="detail-main-title">{{seller.name}}</h1>
					<div class="star-wapper">
						<star :size="48" :score="seller.score"></star>
					</div>
					<div class="detail-title">
						<div class="detail-title-line"></div>
						<div class="detail-title-text">优惠信息</div>
						<div class="detail-title-line"></div>
					</div>
					<div class="detail-msg-wapper">
						<ul v-if="seller.supports" class="detail-msg">
							<li class="detail-msg-item" v-for="item in seller.supports">
								<span class="detail-msg-item-icon" :class="classMap[seller.supports[$index].type]"></span>
								<span class="detail-msg-item-text">{{seller.supports[$index].description}}</span>
							</li>
						</ul>
					</div>
					<div class="detail-title">
						<div class="detail-title-line"></div>
						<div class="detail-title-text">商家公告</div>
						<div class="detail-title-line"></div>
					</div>
					<div class="detail-bulletin-wapper">
						<p class="detail-bulletin">
							{{seller.bulletin}}
						</p>
					</div>
				</div>
			</div>
			<div class="detail-close">
				 <i class="iconfont icon-chahao" @click="hiddenDetail()"></i>
			</div>
		</div>
	</div>
</template>

<script>
import star from 'components/star/star.vue';
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
	components: {
		star
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
		 .decrease{
		 	background: url(decrease_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .discount{
		 	background: url(discount_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .guarantee{
		 	background: url(guarantee_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .invoice{
		 	background: url(invoice_1@2x.png) 0px 0px no-repeat;
		 	background-size: 12px 12px;
		 }
		 .special{
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
			overflow: auto;
			transition: all 0.5s ease;
			backdrop-filter:blur(10px)
		}
		.fade-transition{
			opacity: 1;
			background: rgba(7,17,27,0.8);
		}
		.fade-enter,.fade-leave{
			opacity: 0;
			background: rgba(7,17,27,0);
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
		.star-wapper{
			text-align: center;
			margin-bottom: 28px;
		}
		.detail-main .detail-title{
			display: flex;
			width:80%;
			margin:28px auto 24px auto;
		}
		.detail-main .detail-title-line{
			flex: 1;
			position: relative;
			top:-6px;
			border-bottom: 1px solid rgba(255,255,255,0.2)
		}
		.detail-main .detail-title-text{
			padding:0 12px;
			font-size: 14px;
		}
		.detail-bulletin-wapper{
			width: 80%;
			margin: 0 auto;
		}
		.detail-bulletin{
			padding:0 12px;
			font-size: 12px;
			line-height: 24px;
		}
		.detail-msg-wapper{
			width: 80%;
			margin:0 auto;
		}
		.detail-msg{
			padding: 0 12px;
		}
		.detail-msg-item{
			margin-bottom: 12px;
			font-size: 0;
		}
		.detail-msg-item-icon{
			display: inline-block;
			vertical-align: top;
			margin-right: 6px;
			width: 12px;
			height: 12px;
		}

		.detail-msg-item-text{
			line-height: 12px;
			font-size: 12px;
		}
		.detail-close{
			height: 32px;
			line-height: 32px;
			margin-top: -64px;
			text-align: center;
		}
		.detail-close i{
			color: #fff;
			font-size: 24px;
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