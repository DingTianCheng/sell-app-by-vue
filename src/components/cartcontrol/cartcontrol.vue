<template>
	<div class="cartcontrol">
		<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)" transition="move">
			<span class="inner iconfont icon-jianhao"></span>
		</div>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add iconfont icon-jiahao" @click.stop.prevent="addCart($event)"></div>
	</div>
</template>

<script>
	import Vue from 'vue';
	export default {
		props: {
			food: {
				type: Object
			}
		},
		created () {
			// console.log(this.food);
		},
		methods: {
			addCart (event) {
				if (event._constructed) {
					// console.log(1);
					if (!this.food.count) {
						Vue.set(this.food, 'count', 1);
					} else {
						this.food.count++;
					}
				}
				this.$dispatch('cart.add', event.target);
			},
			decreaseCart (event) {
				if (event._constructed) {
					// console.log(1);
					if (this.food.count === 0) {
						this.food.count = 0;
					} else {
						this.food.count--;
					}
				}
			}
		}
	};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.cartcontrol{
	font-size: 0px;
	.cart-decrease {
		display: inline-block;
		vertical-align:top;
		padding: 6px;
		transition: all 0.4s linear;
		&.move-transition{
			opacity: 1;
			transform: translate3d(0,0,0); 
			.inner{
				display:inline-block;
				width:24px;
				height:24px;
				line-height:24px;
				font-size:24px;
				color:rgb(0,160,220);
				transition: all 0.4s linear;
				transform:rotate(0);
			}
		}
		&.move-enter,&.move-leave{
			opacity: 0;
			transform: translate3d(24px,0,0);
			.inner{
				transform:rotate(180deg);
			}
		}

	}
	.cart-count{
		display: inline-block;
		vertical-align:top;
		padding-top: 6px;
		width: 12px;
		height: 24px;
		line-height:24px;
		text-align: center;
		font-size:10px;
		color:rgb(147,153,159);
	}
	.cart-add{
		display: inline-block;
		vertical-align:top;
		padding: 6px;
		line-height:24px;
		font-size:24px;
		color:rgb(0,160,220);
	}
}
</style>