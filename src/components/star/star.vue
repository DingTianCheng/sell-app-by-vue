<template>
	<div class="star" :class="starType">
		<span v-for="itemCalss in itemClasses" track-by="$index" :class="itemCalss" class="star-item"></span>
	</div>
</template>

<script>
	const LENGTH = 5;
	const CLS_ON = 'on';
	const CLS_HALF = 'half';
	const CLS_OFF = 'off';
	export default {
		props: {
			size: {
				type: Number
			},
			score: {
				type: Number
			}
		},
		computed: {
			starType () {
				return 'star-' + this.size;
			},
			itemClasses () {
				let result = [];
				// 把得来的分数整理，分为 x 或 x.5
				let score = Math.floor(this.score * 2) / 2;
				// 判断是否有余数0.5
				let hasDecimal = score % 1 !== 0;
				// 取整数部分
				let integer = Math.floor(score);
				for (let i = 0; i < integer; i++) {
					result.push(CLS_ON);
				}
				if (hasDecimal) {
					result.push(CLS_HALF);
				}
				while (result.length < LENGTH) {
					result.push(CLS_OFF);
				}
				return result;
			}
		}
	};
</script>

<style scoped>
	div.star{ font-size: 0; }
	div.star span.star-item{ display: inline-block; }

	div.star-48{  }	
	div.star-48 span.star-item{ width: 20px;height: 20px;margin-right: 22px; }
	div.star-48 span.star-item:nth-last-child(1){ margin-right: 0px; }
	div.star-48 span.on{ background: url(images/star48_on@2x.png) 0px 0px no-repeat;background-size:20px 20px; }	
	div.star-48 span.half{ background: url(images/star48_half@2x.png) 0px 0px no-repeat;background-size:20px 20px; }	
	div.star-48 span.off{ background: url(images/star48_off@2x.png) 0px 0px no-repeat;background-size:20px 20px; }

	div.star-36{  }	
	div.star-36 span.star-item{ width: 15px;height: 15px;margin-right: 16px;background-size: 15px 15px; }
	div.star-36 span.star-item:nth-last-child(1){ margin-right: 0px; }
	div.star-36 span.on{ background: url(images/star36_on@2x.png) 0px 0px no-repeat;background-size:15px 15px; }	
	div.star-36 span.half{ background: url(images/star36_half@2x.png) 0px 0px no-repeat;background-size:15px 15px; }	
	div.star-36 span.off{ background: url(images/star36_off@2x.png) 0px 0px no-repeat;background-size:15px 15px; }

	div.star-24{  }
	div.star-24 span.star-item{ width: 10px;height: 10px;margin-right: 3px; }
	div.star-24 span.star-item:nth-last-child(1){ margin-right: 0px; }
	div.star-24 span.on{ background: url(images/star24_on@2x.png) 0px 0px no-repeat;background-size: 10px 10px; }	
	div.star-24 span.half{ background: url(images/star24_half@2x.png) 0px 0px no-repeat;background-size: 10px 10px; }	
	div.star-24 span.off{ background: url(images/star24_off@2x.png) 0px 0px no-repeat;background-size: 10px 10px; }
</style>