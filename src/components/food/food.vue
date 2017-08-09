<template>
	<div class="food" v-show="showFlag" transition="move">
		<div class="food-content">
			<div class="image-header">
				<img :src="food.image" alt="" />
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			food: {
				type: Object
			}
		},
		data () {
			return {
				showFlag: false
			};
		},
		methods: {
			show () {
				this.showFlag = true;
			}
		}
	};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.food{
	position: fixed;
	left: 0px;
	top:0px;
	bottom: 48px;
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
	}
}
</style>