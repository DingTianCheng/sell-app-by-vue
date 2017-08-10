<template>
    <div class="ratings" v-el:ratings>
    	<div class="ratings-content">
    		<div class="overview">
    			<div class="overview-left">
    				<h1 class="score">{{seller.score}}</h1>
    				<div class="title">综合评分</div>
    				<div class="rank">高于周边商家{{seller.rankRate}}%</div>
    			</div>
    			<div class="overview-right">
    				<div class="score-wapper">
    					<span class="title">服务态度</span>
    					<star :size="36" :score="seller.serviceScore"></star>
    					<span class="score">{{seller.serviceScore}}</span>
    				</div>
    				<div class="score-wapper">
    					<span class="title">商品评分</span>
    					<star :size="36" :score="seller.foodScore"></star>
    					<span class="score">{{seller.foodScore}}</span>
    				</div>
    				<div class="delivery-wapper">
    					<span class="title">送达时间</span>
    					<span class="delivery">{{seller.deliveryTime}}分钟</span>
    				</div>
    			</div>
    		</div>
    		<split></split>
    		<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
    		<div class="rating-wrapper">
    			<ul>
    				<li v-for="rating in ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
    					<div class="avatar">
    						<img :src="rating.avatar" width="28" height="28" alt="" />
    					</div>
    					<div class="content">
    						<h1 class="name">{{rating.username}}</h1>
    						<div class="star-wrapper">
    							<star :size="24" :score="rating.score"></star>
    							<span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
    						</div>
    						<p class="text">{{rating.text}}</p>
    						<div class="recommend" v-show="rating.recommend && rating.recommend.length">
    							<span class="iconfont icon-ding"></span>
    							<span class="recommend-item" v-for="item in rating.recommend">{{item}}</span>
    						</div>
    						<div class="time">
    							{{rating.rateTime | formatDate}}
    						</div>
    					</div>
    				</li>
    			</ul>
    		</div>
    	</div>
    </div>
</template>

<script type="ecmascript-6">
    import split from 'components/split/split';
    import star from 'components/star/star';
    import ratingselect from 'components/ratingselect/ratingselect';
    import {formatDate} from 'common/js/date.js';
    import BScroll from 'better-scroll';
    // const POSITIVE = 0;
    // const NEGATIVE = 1;
    const ALL = 2;
    const ERR_OK = 0;
    export default {
    	props: {
    		seller: {
    			type: Object
    		}
    	},
    	data () {
    		return {
    			selectType: ALL,
			onlyContent: false,
			desc: {
				all: '全部',
				positive: '满意',
				negative: '不满意'
			},
			ratings: []
    		};
    	},
    	created () {
    		this.$http.get('/api/ratings').then((response) => {
    			// console.log(response);
    			response = response.body;
    			if (response.errno === ERR_OK){
    				this.ratings = response.data;
    				// console.log(this.ratings);
    				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$els.ratings, {
							click: true
						});
					} else {
						this.scroll.refresh();
					}
				});
    			}
    		});
    	},
    	filters: {
		formatDate (time) {
			let date = new Date(time);
			return formatDate(date, 'yyyy-MM-dd hh:mm');
		}
	},
	events: {
		'ratingtype.select' (type) {
			this.selectType = type;
			this.$nextTick(() => {
				this.scroll.refresh();
			});
		},
		'content.toggle' (onlyContent) {
			this.onlyContent = onlyContent;
			this.$nextTick(() => {
				this.scroll.refresh();
			});
		}
	},
	methods: {
		needShow (type, text) {
			if (this.onlyContent && !text) {
				return false;
			}
			if (this.selectType === ALL) {
				return true;
			} else {
				return type === this.selectType;
			}
		}
	},
    	components: {
    		split,
    		star,
    		ratingselect
    	}
    };
</script>

<style rel="stylesheet/scss" lang="scss" scoped> 
.ratings{
	position: absolute;
	left: 0px;
	top:174px;
	bottom: 0px;
	width: 100%;
	overflow: hidden;
	.overview{
		display: flex;
		padding: 18px 0;
		.overview-left{
			flex:0 0 137px;
			width:137px;
			padding-bottom: 6px;
			border-right: 1px solid rgba(7,17,27,0.1);
			text-align: center;
			@media only screen and (max-width: 320px){
				flex: 0 0 120px;
				width:120px;
			}
			.score{
				line-height: 28px;
				font-size: 24px;
				color: rgb(255,153,0);
			}
			.title{
				margin-bottom: 8px;
				line-height:12px;
				font-size:12px;
				color:rgb(7,17,27);
			}
			.rank{
				line-height:10px;
				font-size:10px;
				color:rgb(147,153,159);
			}
		}
		.overview-right{
			flex:1;
			padding:6px 0  6px 24px;
			@media only screen and (max-width: 320px){
				padding-left: 6px;
			}
			.score-wapper{
				margin-bottom:8px;
				font-size:0px;
				.title{
					display:inline-block;
					line-height:18px;
					vertical-align: top;
					font-size:12px;
					color:rgb(7,17,27)
				}
				.star{
					display:inline-block;
					vertical-align:top;
					margin:0 12px;
					@media only screen and (max-width: 320px){
						margin:0 6px;
					}
				}
				.score{
					display:inline-block;
					line-height:18px;
					vertical-align: top;
					font-size:12px;
					color:rgb(255,153,0);
				}
			}
			.delivery-wapper{
				font-size:0px;
				.title{
					line-height:18px;
					font-size:12px;
					color:rgb(7,17,27);
					margin-right:12px;
					@media only screen and (max-width: 320px){
						margin-right:6px;
					}
				}
				.delivery{
					line-height:18px;
					font-size:12px;
					color:rgb(147,153,159);
				}
			}
		}
	}
	.rating-wrapper{
		padding:0 18px;
		.rating-item{
			display:flex;
			padding:18px 0;
			border-bottom: 1px solid rgba(7,17,27,0.1);
			.avatar{
				flex:0 0 28px;
				width:28px;
				margin-right:12px;
				img{
					border-radius:50%;
				}
			}
			.content{
				flex:1;
				position:relative;
				.name{
					margin-bottom:4px;
					line-height:12px;
					font-size:10px;
					color:rgb(7,17,27);
				}
				.star-wrapper{
					margin-bottom:6px;
					font-size:0px;
					.star{
						display:inline-block;
						vertical-align:top;
						margin-right:6px;
					}
					.delivery{
						display:inline-block;
						vertical-align:top;
						line-height:12px;
						font-size:10px;
						color:rgb(147,153,159);
					}
				}
				.text{
					line-height:18px;
					margin-bottom:8px;
					font-size:9px;
					color:rgb(7,17,27);
				}
				.recommend{
					line-height:16px;
					font-size:0px;
					.icon-ding,.recommend-item{
						display:inline-block;
						vertical-align:top;
						margin:0 8px 4px 0;
					}
					.icon-ding{
						color:rgb(0,160,220);
					}
					.recommend-item{
						padding: 0 6px;
						border:1px solid rgba(7,17,27,0.1);
						border-radius:1px;
						font-size:6px;
						color:rgb(147,153,159);
						background: #fff;
					}
				}
				.time{
					position:absolute;
					top:0px;
					right: 0px;
					line-height:12px;
					font-size:10px;
					color:rgb(147,153,159);
				}
			}
		}
	}
}
</style>