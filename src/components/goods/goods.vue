<template>
<div>


<div class="goods">
	<div class="menu-wrapper" ref="menuWrapper">
		<ul>
			<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
				<span class="text">
					<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
					{{item.name}}
				</span>
			</li>
		</ul>
	</div>
	<div class="foods-wrapper" ref="foodsWrapper">
		 <ul>
          <li v-for="item in goods" class="food-list">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li @click.stop.prevent="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartcontrol :food="food"></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
	</div>
   <shopcart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>

	</div>
    <food :food="selectedFood" ref="food1"></food>
</div>
</template>
<script>
import BScroll from "better-scroll"
import shopcart from "../shopcart/shopcart.vue"
import cartcontrol from "../cartcontrol/cartcontrol.vue"
import food from "../food/food.vue"
export default{
props:{
	seller:{
		type:Object
	}
},
data(){
return{
	goods:[],
    listHeight:[],
    scrolly:0,
    selectedFood:{}
}
},
computed:{
    currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrolly >= height1 && this.scrolly < height2)) {
            // this._followScroll(i);
            return i;
          }
        }
        return 0;
      },
      selectFoods(){
        let foods=[];
        this.goods.forEach((good)=>{
            good.foods.forEach((food)=>{
                if(food.count){
                    foods.push(food);
                }

            });

        });
        return foods;
      }

},

created(){
	 this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	this.$http.get("data.json").then((res)=>{

		this.goods=res.body.goods
		console.log(this.goods);
		this.$nextTick(()=>{
			this._initScroll();

  this.caculateHeight()
      console.log(this.currentIndex)
		})


	})
},
methods:{
    selectMenu(index,$event){
if(!event._constructed){
    return;
}
let foodList=this.$refs.foodsWrapper.getElementsByClassName("food-list");
let el=foodList[index];
this.foodScroll.scrollToElement(el,300);


console.log(index);

    },
    selectFood(food,$event){
        if(!event._constructed){
    return;
}
this.selectedFood=food;
this.$refs.food1.show();

    },
	_initScroll(){
		this.menuScroll=new BScroll(this.$refs.menuWrapper,{
            click:true
        })
		this.foodScroll=new BScroll(this.$refs.foodsWrapper,{
            click:true,
            probeType:3
        });
        this.foodScroll.on("scroll",(pos)=>{
            this.scrolly=Math.abs(Math.round(pos.y));
            // console.log(this.scrolly);


        })
	},
     caculateHeight(){
        let foodsList=this.$refs.foodsWrapper.getElementsByClassName("food-list");
        let height=0;
        this.listHeight.push(height);
        for(let i=0;i<foodsList.length;i++){
            // console.log(foodsList)
            let item =foodsList[i];
            height+=item.clientHeight;
            // console.log(item.clientHeight)
            this.listHeight.push(height);
            // console.log(this.listHeight)


        }
        }
},
components:{
    shopcart,
    cartcontrol,
    food
}


}

</script>
<style lang="stylus">
@import "../../common/stylus/mixins.styl"
.goods
    display:flex
    position:absolute
    width:100%
    top:174px
    bottom:46px
    overflow:hidden
	.menu-wrapper
        flex:0 0 80px
        width:80px
        background:#f3f5f7
        .menu-item
            display:table
            height:54px
            width:56px
            padding:0 12px
            line-height:14px
            &.current
                position:relative
                z-index:10
                color:red
                background:#fff
                font-weight:700
                border-none()
            .text
                display:table-cell
                width:56px
                vertical-align:middle
                // border-1px(rgba(7, 17, 27, 0.1))
                font-size:12px
                .icon
                    display:inline-block
                    width:12px
                    vertical-align:top
                    height:12px
                    margin-right:1px
                    background-size:12px 12px
                .icon.decrease
                    bg-image(decrease_3)
                .icon.discount
                    bg-image(discount_3)
                .icon.special
                    bg-image(special_3)
                .icon.invoice
                    bg-image(invoice_3)
                .icon.guarantee
                    bg-image(guarantee_3)
	.foods-wrapper
		flex:1
		.title
			padding-left: 14px
			height: 26px
			line-height: 26px
			border-left: 2px solid #d9dde1
			font-size: 12px
			color: rgb(147, 153, 159)
			background: #f3f5f7
		.food-item
			display:flex
			margin: 18px
			padding-bottom: 18px
			border-1px(rgba(7, 17, 27, 0.1))
		.food-item:last-child
			border-none()
			margin-bottom:0
		.icon
			flex: 0 0 57px
			margin-right:10px
		.content
			flex:1
			.name
				margin: 2px 0 8px 0
				height: 14px
				line-height: 14px
				font-size: 14px
				color: rgb(7, 17, 27)
			.desc, .extra
				line-height: 10px
				font-size: 10px
				color: rgb(147, 153, 159)
			.desc
				line-height: 12px
				margin-bottom: 8px
			.extra
				.count
					margin-right: 12px
			.price
				font-weight: 700
				line-height: 24px
				.now
					margin-right: 8px
					font-size: 14px
					color: rgb(240, 20, 20)
				.old
					text-decoration: line-through
					font-size: 10px
					color: rgb(147, 153, 159)
	.cartcontrol-wrapper
		position: absolute
		right: 0
		bottom: 12px

</style>