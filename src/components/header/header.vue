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
					<span class="icon"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div
>		</div>
		<!-- 公告 -->
		<div class="bulletin-wapper">
		</div>
	</div>
</template>

<script>
export default {
	props: [ 'seller' ]
};
</script>

<style scoped>
	 .header{
		 color:#fff;
		 background: #000;
		 font-size: 0;
	 }
	/* 内容主体 */
	 @media(min-device-pixel-ratio:1.5),(-webkit-min-device-pixel-ratio:1.5){
			 .title .brand{
				 background: url(brand@3x.png) 0px 0px no-repeat;
				 background-size: 30px 18px;
			 }
		}
	 .content-wapper{
		 padding: 24px 12px 18px 24px;
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
</style>