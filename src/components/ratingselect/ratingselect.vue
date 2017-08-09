<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span class="block positive" :class="{'active':selectType===2}" @click="select(2,$event)">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
			<span class="block positive" :class="{'active':selectType===0}" @click="select(0,$event)">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
			<span class="block negative" :class="{'active':selectType===1}" @click="select(1,$event)">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
		</div>
		<div class="switch" :class="{'on':onlyContent}" @click="toggleContent">
			<span class="iconfont icon-duihao"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
	const POSITIVE = 0;
	const NEGATIVE = 1;
	const ALL = 2;
	export default {
		props: {
			ratings: {
				type: Array,
				default () {
					return [];
				}
			},
			selectType: {
				type: Number,
				default: ALL
			},
			onlyContent: {
				type: Boolean,
				default: false
			},
			desc: {
				type: Object,
				default () {
					return {
						all: '全部',
						positive: '推荐',
						negative: '吐槽'
					};
				}
			}
		},
		computed: {
			positives () {
				return this.ratings.filter((rating) => {
					return rating.ratingType === POSITIVE;
				});
			},
			negatives () {
				return this.ratings.filter((rating) => {
					return rating.ratingType === NEGATIVE;
				});
			}
		},
		methods: {
			select (type, event) {
				if (event._constructed) {
					this.selectType = type;
					this.$dispatch('ratingtype.select', type);
				}
			},
			toggleContent (event) {
				if (event._constructed) {
					this.onlyContent = !this.onlyContent;
					this.$dispatch('content.toggle', this.onlyContent);
				}
			}
		}
	};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.ratingselect{
		.rating-type{
			padding:18px 0;
			margin: 0 18px;
			border-bottom:1px solid rgba(7,17,27,0.1);
			font-size:0px;
			.block{
				display: inline-block;
				vertical-align: top;
				line-height:16px;
				padding:8px 12px;
				border-radius: 2px;
				margin-right:8px;
				color: rgb(77,85,93);
				font-size:12px;
				&.active{
					color:#fff;
				}
				&.positive{
					background: rgba(0,160,220,0.2);
					&.active{
						background: rgb(0,160,220);
					}
				}
				&.negative{
					background: rgba(77,85,93,0.2);
					&.active{
						background: rgb(77,85,93);
					}
				}
				.count{
					font-size:8px;
					margin-left:2px;
				}
			}
		}
		.switch{
			padding:12px 18px;
			font-size:0px;
			line-height:24px;
			border-bottom:1px solid rgba(7,17,27,0.1);
			color:rgb(147,153,159);
			&.on{
				.icon-duihao{
					color:#00c850;
				}
			}
			.icon-duihao{
				display:inline-block;
				vertical-align:top;
				margin-right:4px;
				font-size: 24px;
			}
			.text{
				display:inline-block;
				vertical-align:top;
				font-size:12px;
			}
		}
	}
</style>