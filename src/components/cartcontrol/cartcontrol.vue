<template>
    <div class="cartcontrol">
   <transition name="move">
        <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="desreaseCart">

            <span class="inner icon-remove_circle_outline"></span>
        </div>
    </transition>
        <div class="cart-count" v-show="food.count>0">
            {{food.count}}
        </div>
        <div class="cart-add" @click.stop.prevent="addCart">
            <i class="icon-add_circle"></i>
        </div>
    </div>

</template>
<script>
import Vue from "vue";
    export default{
        props:{
            food:{
                type:Object
            }


        },
        created(){

        },
        methods:{
            addCart(event){
                if(!event._constructed){
                    return;
                }
                if(!this.food.count){

                    Vue.set(this.food,"count",1);
                }else{
                    this.food.count++
                }
            },
            desreaseCart(event){
                      if(!event._constructed){
                    return;
                }
                if(!this.food.count){
                    // this.food.count=1;
                    Vue.set(this.food,"count",1);
                }else{
                    this.food.count--
                }
            },

            }

        }


</script>
<style lang="stylus">
.cartcontrol
    .cart-decrease
        display:inline-block
        padding:4px
        opacity:1
        transition:all 0.4s linear
        .inner
            display:inline-block
            line-height:24px
            font-size:24px
            color:rgb(0,160,220)
            transition:all 0.4s linear
            transform:rotate(0)
        &.move-enter-active,
        &move-leave-active
            transition:all 0.4s linear
        &.move-enter,&.move-leave-active
            opacity:0
            transform:translate3d(24px,0,0)
            .inner
                transform:rotate(180deg)
    .cart-count
        display:inline-block
        padding-top:6px
        line-height:24px
        width:12px
        text-align:center
        font-size:10px
        color:rgb(147,153,159)
        vertical-align:top

    .cart-add
        display: inline-block;
        padding: 6px;
        line-height: 24px;
        font-size: 24px;
        color: #00a0dc;





</style>