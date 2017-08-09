<template>
    <div class="goods">
    	<div class="menu-wapper" v-el:meun-wapper>
    		<ul>
    			<li class="menu-item" :class="{'current':currentIndex===$index}"  v-for="item in goods" @click="selectMeun($index,$event)">
    				<span class="text">
    					<span class="icon" :class="classMap[item.type]" v-show="item.type>0"></span>
    					{{item.name}}
    				</span>
    			</li>
    		</ul>
    	</div>
    	<div class="foods-wapper" v-el:foods-wapper>
    		<ul>
    			<li class="food-list food-list-hook" v-for="item in goods">
    				<h1 class="title">{{item.name}}</h1>
    				<ul>
    					<li class="food-item" v-for="food in item.foods">
    						<div class="icon">
    							<img :src="food.icon" alt="" width="57" height="57";/>
    						</div>
    						<div class="content">
    							<h2 class="name">{{food.name}}</h2>
    							<p class="desc" v-show="food.description">{{food.description}}</p>
    							<div class="extra">
    								<span class="sellCount">月售{{food.sellCount}}份</span>
    								<span>好评率{{food.rating}}%</span>
    							</div>
    							<div class="price">
    								<span class="nowPrice">￥<span>{{food.price}}</span></span>
    								<span class="oldPrice" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
    							</div>
                  <div class="cartcontrol-wapper">
                    <cartcontrol :food="food"></cartcontrol>
                  </div>
    						</div>
    					</li>
    				</ul>
    			</li>
    		</ul>
    	</div>
      <shopcart v-ref:shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    </div>
</template>

<script>
    import BScroll from 'better-scroll';
    import shopcart from 'components/shopcart/shopcart';
    import cartcontrol from 'components/cartcontrol/cartcontrol';
    export default{
    	props: {
    		seller: {
    			type: Object
    		}
    	},
    	data () {
    		return {
    			goods: [],
    			listHeight: [],
    			scrollY: 0
    		};
    	},
    	computed: {
    		currentIndex () {
    			for (let i = 0; i < this.listHeight.length; i++) {
    				let height1 = this.listHeight[i];
    				let height2 = this.listHeight[i + 1];
    				if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
    					return i;
    				}
    			}
    			return 0;
    		},
           selectFoods () {
              let selectFoods = [];
              this.goods.forEach((good) => {
                good.foods.forEach((food) => {
                  if (food.count) {
                    selectFoods.push(food);
                  }
                });
              });
              return selectFoods;
           }
    	},
    	created () {
    		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		this.$http.get('/api/goods').then((response) => {
			// console.log(response);
			response = response.body;
			const ERR_OK = 0;
			if (response.errno === ERR_OK) {
				let data = response.data;
				this.goods = data;
				// console.log(this.goods);
				this.$nextTick(() => {
					this._initScroll();
					this._calculateHeight();
				});
			}
		});
    	},
    	methods: {
		_initScroll () {
    			this.meunScroll = new BScroll(this.$els.meunWapper, {
    				click: true
    			});
    			this.foodsScroll = new BScroll(this.$els.foodsWapper, {
                     click: true,
    				probeType: 3
    			});
    			this.foodsScroll.on('scroll', (pos) => {
    				this.scrollY = Math.abs(Math.round(pos.y));
    				// console.log(this.scrollY);
    			});
    		},
    		_calculateHeight () {
    			let foodList = this.$els.foodsWapper.getElementsByClassName('food-list-hook');
    			let height = 0;
    			this.listHeight.push(height);
    			for (var i = 0; i < foodList.length; i++) {
    				let item = foodList[i];
    				height += item.clientHeight;
    				this.listHeight.push(height);
    			}
    			// console.log(this.listHeight);
    		},
    		selectMeun (index, event) {
    			if (event._constructed) {
    				// console.log(index);
    				let foodList = this.$els.foodsWapper.getElementsByClassName('food-list-hook');
    				let el = foodList[index];
    				this.foodsScroll.scrollToElement(el, 300);
    			}
    		},
          _drop (target) {
            // 体验优化，异步执行下落动画
            this.$nextTick(() => {
              this.$refs.shopcart.drop(target);
            });
          }
    	},
      components: {
        shopcart,
        cartcontrol
      },
      events: {
        'cart.add' (target) {
          this._drop(target);
        }
      }
    };
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.goods {
	position: absolute;
	top:174px;
	bottom: 46px;
	width:100%;
	display:-webkit-flex;/* Safari */
  	display: flex;
  	overflow: hidden;
  	.menu-wapper{
  		flex:0 0 80px;
  		width:80px;/* 防止出现安卓对flex的不兼容问题 */
  		background: #f3f5f7;
  		.menu-item{
  			display:table;
  			height:54px;
  			width:80px;
  			line-height:14px;
  			font-size:12px;
  			&.current{
  				position:relative;
  				z-index: 10;
  				margin-top: -1px;
  				background:#fff;
  				font-weight:700;
  				.text{
  					border-bottom:none;
  				}
  			}
  			.text{
  				display:table-cell;
  				width:56px;
  				vertical-align:middle;
	  			font-size: 12px;
	  			border-bottom: 1px solid rgba(7,17,27,0.1);
	  			padding: 0px 12px;
	  		}
  			.icon{
  				display:inline-block;
  				vertical-align:top;
  				width:12px;
  				height: 12px;
  				margin-right: 2px;
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
  		}	
  	}
  	.foods-wapper{
  		flex:1;
  		.title{
  			padding-left: 14px;
  			height:26px;
  			line-height:26px;
  			border-left: 2px solid #d9dde1;
  			background:#f3f5f7;
  			font-size:12px;
  			color:rgb(147,153,159);	
  		}
  		.food-item{
  			display:flex;
  			margin:18px;
  			padding-bottom: 18px;
  			border-bottom: 1px solid rgba(7,17,27,0.1);
  			&:nth-last-child(1){
  				border-bottom:none;
  				margin-bottom: 0px;
  			}
  			.icon{
  				flex:0 0 57px;
  				margin-right:10px;
  			}
  			.content{
  				 flex:1;
                      position:relative;
  				.name{
  					margin:2px 0px 8px 0;
  					height:14px;
  					line-height:14px;
  					font-size:14px;
  					color:rgb(7,17,27);
  				}
  				.desc,.extra{
  					margin-bottom:8px;
  					line-height:10px;
  					font-size:10px;
  					color:rgb(147,153,159);
  				}
  				.extra{
  					font-size:0px;
  					margin-bottom:0px;
  					span{
  						font-size:10px;
  						&.sellCount{
  							 margin-right:12px;
  						}	
  					}
  				}
  				.price{
  					font-size:0px;
  					.nowPrice{
	  					font-size:10px;
	  					height:24px;
	  					line-height:24px;
	  					font-weight: 700;
	  					line-height:24px;
	  					color:red;
	  					margin-right:8px;
	  					span{
	  						font-size:14px;
	  					}
	  				}
	  				.oldPrice{
	  					line-height:24px;
	  					font-size:10px;
	  					color:rgb(147,153,159);
	  					font-weight:700;
	  					text-decoration: line-through;
	  				}
  				}
                      .cartcontrol-wapper{
                          position:absolute;
                          right: 0px;
                          bottom:-6px;
                      }
  			}
  		}
  	}
}
</style>