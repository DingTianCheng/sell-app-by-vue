<template>
    <div class="seller">
    	<div class="seller-content">
    		<div class="overview">
    			<h1 class="title">{{seller.name}}</h1>
    			<div class="desc">
    				<star :size="36" :score="seller.score"></star>
    				<span class="text">({{seller.ratingCount}})</span>
    				<span class="text">月售{{seller.sellCount}}单</span>
    			</div>
    			<ul class="remark">
    				<li class="block">
    					<h2>起送价</h2>
    					<div class="content">
    						<span class="stress">{{seller.minPrice}}</span>元
    					</div>
    				</li>
    				<li class="block">
    					<h2>商家配送</h2>
    					<div class="content">
    						<span class="stress">{{seller.deliveryPrice}}</span>元
    					</div>
    				</li>
    				<li class="block">
    					<h2>平均配送时间</h2>
    					<div class="content">
    						<span class="stress">{{seller.deliveryTime}}</span>分钟
    					</div>
    				</li>
    			</ul>
    		</div>
    		<split></split>
    		<div class="bulletin">
    			<h1 class="title">公告与活动</h1>
    			<div class="content-wrapper">
    				<p class="content">{{seller.bulletin}}</p>
    			</div>
    		</div>
    		<div class="detail-msg-wapper">
			<ul v-if="seller.supports" class="detail-msg">
				<li class="detail-msg-item" v-for="item in seller.supports">
					<span class="detail-msg-item-icon" :class="classMap[seller.supports[$index].type]"></span>
					<span class="detail-msg-item-text">{{seller.supports[$index].description}}</span>
				</li>
			</ul>
		</div>
		<split></split>
    	</div>
    </div>
</template>

<script type="ecmascript-6">
    import split from 'components/split/split';
    import star from 'components/star/star';
    import BScroll from 'better-scroll';
    export default {
    	props: {
    		seller: {
    			type: Object
    		}
    	},
    	created () {
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	},
    	components: {
    		split,
    		star
    	}
    };
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.seller{
	position: absolute;
	top:174px;
	bottom: 0px;
	left: 0px;
	width: 100%;
	overview:hidden;
	.overview{
		padding: 18px;
		.title{
			margin-bottom:8px;
			line-height: 14px;
			color: rgb(7,17,27);
			font-size: 14px;
		}
		.desc{
			padding-bottom:18px;
			font-size: 0px;
			border-bottom: 1px solid rgba(7,17,27,0.1);
			.star{
				display: inline-block;
				margin-right:8px;
				vertical-align:top;
			}
			.text{
				display: inline-block;
				line-height:18px;
				margin-right:12px;
				vertical-align:top;
				font-size:10px;
				color:rgb(77,85,93);
			}
		}
		.remark{
			display:flex;
			padding-top:18px;
			.block{
				flex:1;
				text-align: center;
				border-right:1px solid rgba(7,17,27,0.1);
				&:nth-last-child(1){
					border-right:none;
				}
				h2{
					margin-bottom:4px;
					line-height:10px;
					font-size:10px;
					color:rgb(147,153,159);
				}
				.content{
					line-height:24px;
					font-size:10px;
					color:rgb(7,17,27);
					.stress{
						font-size:24px;
					}
				}
			}
		}
	}
	.bulletin{
		padding:18px 18px 0px 18px;
		.title{
			margin-bottom:8px;
			line-height: 14px;
			color: rgb(7,17,27);
			font-size: 14px;	
		}
		.content-wrapper{
			padding:0px 12px 16px 12px;
			border-bottom:1px solid rgba(7,17,27,0.1);
			.content{
				line-height:24px;
				font-size:12px;
				color:rgb(240,20,20);
			}
		}
	}
	.detail-msg-wapper{
		padding:0px 18px 0px 18px;
		.detail-msg-item{
			padding:16px 12px;
			font-size:0px;
			border-bottom:1px solid rgba(7,17,27,0.1);
			&:nth-last-child(1){
				border-bottom:none;
			}
			.detail-msg-item-icon{
				display: inline-block;
			 	vertical-align: top;
			 	width: 12px;
			 	height: 12px;
				 &.decrease{
				 	background: url(images/decrease_3@2x.png) 0px 0px no-repeat;
				 	background-size: 12px 12px;
				 }
				 &.discount{
				 	background: url(images/discount_3@2x.png) 0px 0px no-repeat;
				 	background-size: 12px 12px;
				 }
				 &.guarantee{
				 	background: url(images/guarantee_3@2x.png) 0px 0px no-repeat;
				 	background-size: 12px 12px;
				 }
				 &.invoice{
				 	background: url(images/invoice_3@2x.png) 0px 0px no-repeat;
				 	background-size: 12px 12px;
				 }
				 &.special{
				 	background: url(images/special_3@2x.png) 0px 0px no-repeat;
				 	background-size: 12px 12px;
				 }
			}
			.detail-msg-item-text{
				margin-left: 4px;
			 	line-height: 12px;
			 	font-size: 10px;
			}
		}
	}
}
</style>