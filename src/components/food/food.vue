<template>
	<div class="food" v-show="showFlag" transition="move" v-el:food>
		<div class="food-content">
			<div class="image-header">
				<img :src="food.image" alt="" />
				<div class="back" @click="hide">
					<i class="iconfont icon-fanhuianniu"></i>
				</div>
			</div>
			<div class="content">
				<h1 class="title">{{food.name}}</h1>
				<div class="detail">
					<span class="sell-count">月售{{food.sellCount}}份</span>
					<span class="rating">好评率{{food.rating}}</span>
				</div>
				<div class="price">
					<span class="now">￥{{food.price}}</span>
					<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
				</div>
				<div class="cartcontrol-wapper">
					<cartcontrol :food="food"></cartcontrol>
				</div>
				<div class="buy" v-show="!food.count||food.count===0" @click.stop.prevent="addFirst" transition="fade">
					加入购物车
				</div>
			</div>
			<split v-show="food.info"></split>
			<div class="info" v-show="food.info">
				<h1 class="title">商品信息</h1>
				<p class="text">{{food.info}}</p>
			</div>
			<split v-show="food.info"></split>
			<div class="rating">
				<h1 class="title">商品评价</h1>
				<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
				<div class="rating-wapper">
					<ul v-show="food.ratings && food.ratings.length">
						<li class="rating-item" v-for="rating in food.ratings" v-show="needShow(rating.rateType,rating.text)">
							<div class="user">
								<span class="name">{{rating.username}}</span>
								<img class="avatar" width="12" height="12" :src="rating.avatar" alt="" />
							</div>
							<div class="time">{{rating.rateTime | formatDate}}</div>
							<div class="text">
								<span class="iconfont" :class="{ 'icon-ding':rating.rateType===0,'icon-cai':rating.rateType===1 }"></span>
								{{rating.text}}
							</div>
						</li>
					</ul>
					<div class="no-rating" v-show="!food.ratings || !food.ratings.length">
						暂无评价
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import BScroll from 'better-scroll';
	import cartcontrol from 'components/cartcontrol/cartcontrol';
	import Vue from 'vue';
	import split from 'components/split/split';
	import ratingselect from 'components/ratingselect/ratingselect';
	import {formatDate} from 'common/js/date.js';
	// const POSITIVE = 0;
	// const NEGATIVE = 1;
	const ALL = 2;
	export default {
		props: {
			food: {
				type: Object
			}
		},
		data () {
			return {
				showFlag: false,
				selectType: ALL,
				onlyContent: false,
				desc: {
					all: '全部',
					positive: '推荐',
					negative: '吐槽'
				}
			};
		},
		methods: {
			show () {
				this.showFlag = true;
				this.selectType = ALL;
				this.onlyContent = false;
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$els.food, {
							click: true
						});
					} else {
						this.scroll.refresh();
					}
				});
			},
			hide () {
				this.showFlag = false;
			},
			addFirst (event) {
				if (event._constructed) {
					this.$dispatch('cart.add', event.target);
					Vue.set(this.food, 'count', 1);
				}
			},
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
		filters: {
			formatDate (time) {
				let date = new Date(time);
				return formatDate(date, 'yyyy-MM-dd hh:mm');
			}
		},
		components: {
			cartcontrol,
			split,
			ratingselect
		}
	};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.food{
	position: fixed;
	left: 0px;
	top:0px;
	bottom:48px;
	z-index: 30;
	width: 100%;
	background: #fff;
	&.move-transition{
		transition:all 0.2s linear;
		transform:translate3d(0,0,0);
	}
	&.move-enter,&.move-leave{
		transform:translate3d(100%,0,0);
	}
	.image-header{
		position:relative;
		width:100%;
		height: 0;
		padding-top:100%;
		img{
			position:absolute;
			top:0px;
			left:0px;
			width:100%;
			height:100%;
		}
		.back{
			position:absolute;
			top:10px;
			left:0px;
			.icon-fanhuianniu{
				display: block;
				padding: 10px;
				font-size: 20px;
				color: #fff;
			}
		}
	}
	.content{
		padding:18px;
		position:relative;
		.title{
			line-height: 14px;
			margin-bottom: 8px;
			font-size:14px;
			color:rgb(7,17,27);
			font-weight: 700;
		}
		.detail{
			margin-bottom:18px;
			height:10px;
			line-height:10px;
			font-size:0;
			.sell-count,.rating{
				font-size:10px;
				color:rgb(147,153,159);
			}
			.sell-count{
				margin-right: 12px;
			}
		}
		.price{
			font-weight:700;
			line-height:24px;
			font-size:0px;
			.now{
				margin-right:8px;
				font-size:14px;
				color:rgb(240,20,20);
			}
			.old{
				text-decoration: line-through;
				font-size:10px;
				color:rgb(147,153,159);
			}
		}
	}
	.cartcontrol-wapper{
		position:absolute;
		right:12px;
		bottom:12px;
	}
	.buy{
		position:absolute;
		right:18px;
		bottom:18px;
		z-index:10;
		height:24px;
		line-height:24px;
		text-align: center;
		padding:0 12px;
		box-sizing:border-box;
		border-radius: 12px;
		font-size:10px;
		color:#fff;
		background:rgb(0,100,220);
		&.fade-transtion{
			transition:all 0.5s;
			opacity: 1;
		}
		&.fade-enter,&.fade-leave{
			opacity: 0;
		}
	}
	.info{
		padding:18px;
		.title{
			line-height:14px;
			margin-bottom:6px;
			font-size:14px;
			color:rgb(7,17,27);
		}
		.text{
			line-height:24px;
			padding:0 8px;
			font-size:12px;
			color:rgb(77,85,93);
		}
	}
	.rating{
		padding-top:18px;
		.title{
			line-height:14px;
			margin-left:18px;
			font-size:14px;
			color:rgb(7,17,27);
		}
		.rating-wapper{
			margin:0 18px;
			.rating-item{
				position:relative;
				padding:16px 0;
				border-bottom:1px solid rgba(7,17,27,0.1);
				.user{
					position:absolute;
					right:0px;
					top:16px;
					line-height:12px;
					font-size:0px;
					.name{
						display:inline-block;
						vertical-align: top;
						margin-right:6px;
						font-size:10px;
						color:rgb(147,153,159);
					}
					.avatar{
						border-radius:50%;
					}
				}
				.time{
					line-height:12px;
					margin-bottom:6px;
					font-size:10px;
					color:rgb(147,153,159);
				}
				.text{
					line-height:16px;
					font-size:12px;
					color:rgb(7,17,27);
					.iconfont{
						margin-right:4px;
						line-height:24px;
						font-size:12px;
						&.icon-ding{
							color:rgb(0,160,220);
						}
						&.icon-cai{
							color:rgb(147,153,159);
						}
					}
				}	
			}
			.no-rating{
				padding:16px 0;
				font-size:12px;
				color:rgb(147,153,159);
			}
		}
	}
}
</style>