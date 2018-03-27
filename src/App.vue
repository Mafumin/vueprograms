<template>
	<div>
		<v-header :seller="seller"></v-header>
		<div class="tab">
			<div class="tab-item">
				<router-link :to="{name:'goods'}" >商品</router-link>
			</div>
			<div class="tab-item">
				<router-link :to="{name:'ratings'}">评论</router-link>
			</div>
			<div class="tab-item">
				<router-link :to="{name:'seller'}">商家</router-link>
			</div>

		</div>
		<router-view  :seller="seller"></router-view>

	</div>
</template>

<script>
import header from './components/header/header.vue'
const err_ok=0;
export default{

  data(){
return {
  seller:{}
}
  },
  components:{
"v-header":header
  },
  created(){
this.$http.get("../data.json").then(res=>{
// console.log(res.body.seller)
// res=res.body.seller;
// if(res.errno===err_ok){
  this.seller=res.body.seller;
  console.log(this.seller);
// }

})
    }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "./common/stylus/mixins.styl"
.tab
	display:flex
	width:100%
	line-height:40px
	height:40px
	border-1px(rgba(7,17,27,0.1))

	&>.tab-item
		flex:1
		display:inline-block
		text-align:center


</style>
