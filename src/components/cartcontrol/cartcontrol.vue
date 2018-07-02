<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease" @click.stop.prevent="decreaseCart" v-show="food.count > 0">
                <div class="inner iconfont icon-jianhao"></div>
            </div>
        </transition>
        <div class="cart-count"  v-show="food.count > 0">{{food.count}}</div>
        <div class="crat-add iconfont icon-jiahao" @click.stop.prevent="addCart"></div>
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
        created() {
            // console.log(this.food);
            // console.log(this.$poprs)
        },
        methods: {
            addCart(event) {
                if(event.__constructed) {
                    return;
                }
                if(!this.food.count) {
                    // 全局设置 this.food对象 添加 count 属性 默认值为1;
                    Vue.set(this.food,'count',1);
                }else {
                    this.food.count ++;
                }

                // 派发事件
                this.$emit('add',event.target);
            },
            decreaseCart(event) {
                if(event.__constructed) {
                    return;
                }
                if(this.food.count) { 
                    this.food.count --;
                }
            }


            

        }
    };
</script>

<style lang="scss">

    .move-enter-active,.move-leave-active {
        opacity: 1;
        transform: rotate(180deg);
        transform: translate3d(0,0,0);   
    }
    .move-enter, .move-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0;
        transform: rotate(0);
        transform: translate3d(24px,0,0);    
    }


    .cartcontrol {
        font-size: 0;
        .cart-decrease {
            display: inline-block;
            padding: 0.16rem;
            line-height: 0.64rem;
            color: rgb(0,160,220);
            transition: all 0.4s linear;
            .inner {
                line-height: 0.64rem;
                font-size: 20px;
                transition: all 0.4s linear;
            }
        }
        .cart-count {
            display: inline-block;
            vertical-align: top;
            width: 0.32rem;
            padding-top: 0.16rem;
            line-height: 0.64rem;
            text-align: center;
            font-size: 12px;
            color: rgb(147,153,159);
        }
        .crat-add {
            display: inline-block;
            padding: 0.16rem;
            line-height: 0.64rem;
            font-size: 20px;
            color: rgb(0,160,220)
        }
    }
</style>
