<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" width="64" height="64" alt="">

			</div>
			<div class="content">
				<div class="title">
					<span class="brand1"></span>
					<span class="name">{{seller.name}}</span>

				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达

				</div>
				<div v-if="seller.supports" class="support">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>


				</div>

			</div>
			<div v-if="seller.supports" class="support-count" @click="showDetail()">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>

		</div>
		<div class="bulletin-wrapper" @click="showDetail()">
				<span class="bulletin-title"></span>
				<span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-clubs"></i>
		</div>
		<div class="background">
		<img :src="seller.avatar" alt="" width="100%" height="100%">

		</div>
		<div v-show="detailShow" class="detail" transition="fade">
		<div class="detail-wrapper clearfix">
<div class="detail-main">
	<h1 class="name">{{seller.name}}</h1>
	<star :size="48" :score="seller.score" class="star-wrapper"></star>
	<div class="title">
	<div class="line"></div>
	<div class="text">优惠信息</div>
	<div class="line"></div>
	</div>
	<ul v-if="seller.supports" class="supports">
		<li class="support-item" v-for="(item,index) in seller.supports">
			<span class="icon" :class="classMap[seller.supports[index].type]"></span>
			<span class="text">{{seller.supports[index].description}}</span>
		</li>
	</ul>
	<div class="title">
	<div class="line"></div>
	<div class="text">商家公告</div>
	<div class="line"></div>
	</div>
	<div class="bulletin">
	 <p class="content">{{seller.bulletin}}</p>

	</div>

</div>


		</div>
		<div class="detail-close" @click="hideDetail()">
			<i class="icon-spades"></i>
		</div>

		</div>

	</div>

</template>
<script>
import star from "../star/star.vue";

export default{
	props:{
seller:{
	type:Object
	}
},
created(){
	 this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
},
data(){
	return{
		detailShow:false
	}

},
methods:{
	showDetail(){
		this.detailShow=true
	},
	hideDetail(){
		this.detailShow=false
	}

},
components:{
	star
}


}

</script>
<style lang="stylus">
@import "../../common/stylus/mixins"

.header
	position:relative
	color:#fff
	overflow:hidden
	//hahh rjg gjioj
	background:rgba(7,17,27,0.5)
	.content-wrapper
		position:relative
		padding:24px 12px 18px 24px
		font-size:0
		.avatar
			display:inline-block
			vertical-align:top
			img
				border-radius:2px
		.content
			display:inline-block
			font-size:14px
			margin-left:16px
			 .title
				margin:2px 0 8px 0
				 .brand1
					display:inline-block
					vertical-align:top
					width:30px
					height:18px
					background-size:30px 18px
					bg-image(brand)
					background-repeat:no-repeat
				.name
					margin-left:6px
					font-size:16px
					line-height:18px
					font-weight:bold

			.description
				margin-bottom:10px
				line-height:12px
				font-size:12px
			.support
				.icon
					display:inline-block
					width:16px
					vertical-align:top
					height:16px
					margin-right:4px
					background-size:16px 16px
	           	.icon.decrease
					bg-image(decrease_1)
	           	.icon.discount
					bg-image(discount_1)
	           	.icon.special
					bg-image(special_1)
	           	.icon.invoice
					bg-image(invoice_1)
	           	.icon.guarantee
					bg-image(guarantee_1)

				.text
					font-size:12px
					line-height:16px
		.support-count
			position: absolute;
			right: 12px;
			bottom: 14px;
			padding: 0 8px;
			height: 24px;
			line-height: 24px;
			border-radius: 14px;
			width:40px
			background-color: red;
			text-align: center;
                .count
					vertical-align: top
					font-size: 12px
				.icon-keyboard_arrow_right

					margin-left:2px;
					line-height: 24px;
					font-size: 12px;


	.bulletin-wrapper
		position:relative
		height:28px
		line-height:28px
		padding-right:12px
		padding-left:22px
		white-space:nowrap
		overflow:hidden
		text-overflow:ellipsis
		font-size:12px
		.bulletin-title
			display:inline-block
			width:22px
			height:12px
			bg-image(bulletin)
			background-size:22px 12px
			background-repeat:no-repeat
		.bulletin-text {
		    vertical-align: top;
		    margin: 0 4px;
		    font-size: 12px;
		}
		.icon-clubs{
		        position: absolute;
		        font-size: 10px;
		        right: 8px;
		        top: 4px;

		    }
	.background
		position:absolute
		top:0
		left:0
		width:100%
		height:100%
		z-index:-1
		filter:blur(10px)
	.detail
		position:fixed
		z-index:100
		top:0
		left:0
		width:100%
		height:100%
		overflow:auto
		background:rgba(7,17,27,0.8)
		.detail-wrapper
			min-height:100%
			width:100%
			.detail-main
				margin-top:64px
				padding-bottom:64px
				.name
					line-height:16px
					height:16px
					text-align:center
					font-size:16px;
					font-weight:700
			 	.star-wrapper
					margin-top:18px
					padding:2px 0
					text-align:center
				.title
					display:flex
					width:80%
					margin:30px auto 24px auto
					.line
						flex:1
						position:relative
						top:-6px
						border-bottom:1px solid rgba(255,255,255,0.2)
					.text
						padding:0 12px
						font-size:14px
						font-weight:700

				.supports
					width:80%
					margin:0 auto
					.support-item
						padding:0 12px
						margin-bottom:12px
						font-size:0
					.support-item:last-child
						margin-bottom:0
						.icon
							display:inline-block
							width: 16px
							height: 16px
							vertical-align: top
							margin-right: 6px
							background-size: 16px 16px
							background-repeat: no-repeat
						.icon.decrease
							bg-image(decrease_2)
						.icon.discount
							bg-image(discount_2)
						.icon.special
							bg-image(special_2)
						.icon.invoice
							bg-image(invoice_2)
						.icon.guarantee
							bg-image(guarantee_2)
						.text
							line-height: 16px
							font-size: 12px
				.bulletin
					width:80%
					margin:0 auto
					.content
						padding:0 12px
						line-height:24px
						font-size:12px

		.detail-close
			position:relative
			width:32px;
			height:32px;
			margin:-100px auto 0 auto
			clear:both
			font-size:32px



</style>