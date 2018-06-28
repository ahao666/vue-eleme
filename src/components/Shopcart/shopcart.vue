<template>
    <div class="shopcat">
        <div class="content" @click="toggleList">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight': totalCount > 0}">
                        <i class="iconfont icon-gouwuche" :class="{'highlight': totalCount > 0}"></i>
                    </div>
                    <div class="num" v-if="totalCount">{{totalCount}}</div>
                </div>

                <div class="price" :class="{'highlight': totalPrice > 0}">¥{{totalPrice}}</div>
                <div class="desc">另需配送费¥{{deliveryPrice}}元</div>

            </div>
            <div class="content-right" @click.stop.prevent="pay">
                <div class="pay" :class="payClass">
                    {{payDesc}}
                </div>
            </div>
        </div>
        <div class="ball-contariner">
            <div v-for="(ball,index) in balls" :key="index">
                <transition  name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
                <div class="ball" v-show="ball.show" >
                    <div class="inner inner-hook"></div>    
                </div>
            </transition>
            </div>
        </div>

        <transition name="fold">
            <div class="shopcart-list" v-show="listShow">
                <div class="list-header">
                    <h1 class="title">购物车</h1>
                    <span class="empty" @click="empty">清空</span>
                </div>
                <div class="list-content" ref="listcontent">
                    <ul>
                        <li class="food" v-for="(food,index) in selectFood" :key="index">
                            <span class="name">{{food.name}}</span>
                            <div class="price">
                                <span>¥{{food.price*food.count}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </transition>

        <div class="list-mask" @click="hideList" v-show="listShow">
        
        </div>
    </div>
   

</template>

<script>
    import cartcontrol from '../cartcontrol/cartcontrol.vue';

    import BScroll from 'better-scroll';

    export default {
        props: {
            selectFood: {
                type: Array,
                default() {
                    return [
                        {
                            price: 0,
                            count: 0
                        }
                    ];
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
        },
        components: {
            cartcontrol
        },
        data() {
            return {
                balls: [
                    {
                        show: false,
                    },
                    {
                        show: false,
                    },
                    {
                        show: false,
                    },
                    {
                        show: false,
                    },
                    {
                        show: false,
                    },
                ],
                dropBalls: [],
                fold: true
            }
        },
        computed: {
            totalPrice() {  // 计算 总价格  单价 * 数量
                let total = 0;
                this.selectFood.forEach((food,index)=>{
                    total += food.price * food.count;
                })
                return total;
            },
            totalCount() { // 计算 数量
                let count = 0;
                 this.selectFood.forEach((food,index)=>{
                    count += food.count;
                })
                return count;
            },
            payDesc() {
                if(this.totalPrice === 0) {
                    return `¥${this.minPrice}元起送`;
                }else if(this.totalPrice < this.minPrice) {
                    let diff = this.minPrice - this.totalPrice;
                    return `还差¥${diff}元`;
                }else {
                    return `去结算`;
                }
            },
            payClass() {
                if(this.totalPrice < this.minPrice) {
                    return 'not-enough';
                }else {
                    return 'enough';
                }
            },
            listShow() {
                if(!this.totalCount) {
                    this.fold = true;
                    return false;
                }
                let show = !this.fold; 
                if(show) {
                    this.$nextTick(()=> {
                        if(!this.scroll) {
                            this.scroll = new BScroll(this.$refs.listcontent,{
                                click: true
                            })
                        }else {
                            this.scroll.refresh();  // 重新计算 better-scroll，当 DOM 结构发生变化的时候务必要调用确保滚动的效果正常。
                        }
                       
                    })
                } 
                return show;
            }
        },
        methods: {
            drop(el) {
                // console.log(el);
                for(let i=0;i<this.balls.length;i++) {
                    let ball = this.balls[i];
                    if(!ball.show) {
                        ball.show = true;
                        ball.el = el;
                        this.dropBalls.push(ball);
                        return;
                    }
                }
            },
            beforeDrop(el) {
                // console.log(el);
                let count = this.balls.length;  // balls的长度
                while (count--) {
                    let ball = this.balls[count];  // balls数组中的对象
                    if(ball.show === true) {
                        let rect = ball.el.getBoundingClientRect();
                        let x = rect.left - 32; // 小球的左边偏移  ball left: 32px;
                        let y = - (window.innerHeight - rect.top -22);
                        el.style.display = "";
                        el.style.wibkitTransform = `translate3d(0,${y}px,0)`;
                        el.style.transform = `translate3d(0,${y}px,0)`;
                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
                        inner.style.transform = `translate3d(${x}px,0,0)`;
                    }
                }
            },
            dropping(el) {
                let rf = el.offsetHeight;
                this.$nextTick(() => {
                    el.style.webkitTransform = 'translate3d(0,0,0)';
                    el.style.transform = 'translate3d(0,0,0)';
                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webkitTransform = 'translate3d(0,0,0)';
                    inner.style.transform = 'translate3d(0,0,0)';
                });
            },
            afterDrop(el) {
                let ball = this.dropBalls.shift();
                if(ball) {
                    ball.show = false;
                    el.style.display = "none";
                }
            },
            toggleList() {
                if(!this.totalCount) {
                    return 
                }
                this.fold = !this.fold;
            },
            empty() {
                this.selectFood.forEach((food,index)=>{
                    food.count = 0;
                })
            },
            hideList() {
                this.fold = true;
            },
            pay() {  // 结算
                if(this.totalPrice < this.minPrice) {
                    return;
                }else {
                    window.alert(`支付${this.totalPrice}元`);
                }
            }
        }
    }
</script>

<style lang="scss">

    .fold-enter-active, .fold-leave-active {
        transform: translate3d(0,-100%,0);
    }
    .fold-enter, .fold-leave-to /* .fade-leave-active below version 2.1.8 */ {
        transform: translate3d(0,0,0);
    }
    
    .shopcat {
        position: fixed;
        left: 0;
        bottom: 0;
        z-index: 999;
        width: 100%;
        height: 1.28rem;
        .content {
            display: flex;
            font-size: 0;
            background: #141d27;
            .content-left {
                flex: 1;
                .logo-wrapper {
                    display: inline-block;
                    position: relative;
                    top: -0.25rem;
                    margin: 0 0.32rem;
                    padding: 0.16rem;
                    width: 1.5rem;
                    height: 1.5rem;
                    box-sizing: border-box;
                    vertical-align: top;
                    border-radius: 50%;
                    background-color: #142d27;
                    .logo {
                        width: 100%;
                        height: 100%;
                        border-radius: 50%;
                        background: #2b343c;
                        text-align: center;
                        &.highlight {
                            background:rgb(0,160,220);
                        }
                        i {
                            font-size: 24px;
                            line-height: 1.2rem;
                            font-style: normal;
                            color: rgba(255,255,255,0.4);
                            &.highlight {
                                 color: #fff;
                            }
                        }
                    }
                    .num {
                        position: absolute;
                        top: 0;
                        right: 0;
                        width: 0.64rem;
                        height: 0.42rem;
                        line-height: 0.42rem;
                        text-align: center;
                        border-radius: 16px;
                        font-size: 9px;
                        font-weight: 700;
                        color: #fff;
                        background-color: rgb(240,20,20);
                        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);                        
                    }
                }
                .price {
                    display: inline-block;
                    vertical-align: top;
                    margin-top: 0.32rem;
                    padding-right: 0.32rem;
                    line-height: 0.64rem;
                    box-sizing: border-box;
                    border-right: 1px solid rgba(255,255,255,0.1);
                    font-size: 16px;
                    font-weight: 700;
                    color: rgba(255,255,255,0.4);
                    &.highlight {
                        color: #fff;
                    }
                }
                .desc {
                    display: inline-block;
                    vertical-align: top;
                    margin: 0.32rem 0 0 0.32rem;
                    line-height: 0.64rem;
                    color: rgba(255,255,255,0.4);
                    font-size: 10px;
                }
            }
            .content-right {
                flex: 0 0 2.8rem;
                width: 2.8rem;
                .pay {
                    height: 1.28rem;
                    line-height: 1.28rem;
                    text-align: center;
                    font-size: 12px;
                    font-weight: 700;
                    color: rgba(255,255,255,0.4);
                    background: #2b333b;
                    &.not-enough {
                        background-color: #2b333b;
                    }
                    &.enough {
                        background-color: #00b43c;
                        color: #fff;
                    }
                }
            }
        }
        .ball-contariner {
            .ball {
                position: fixed;
                right: 32px;
                bottom: 22px;
                z-index: 1999;
                transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41);
                .inner {
                    width: 16px;
                    height: 16px;
                    border-radius: 50%;
                    background-color: rgb(0,160,220);
                }
            }
        }
        .shopcart-list {
            position: absolute;
            top: 0;
            left: 0;
            z-index: -1;
            width: 100%;
            background-color: red;
            transform: translate3d(0,-100%,0);
            .list-header {
                height: 40px;
                line-height: 40px;
                padding: 0 18px;
                background-color: #f3f5f7;
                border-bottom: 1px solid rgba(7,17,27,0.1);
                .title {
                    float: left;
                    font-size: 14px;
                    color: rgba(7,17,27,1);
                }
                .empty {
                    float: right;
                    font-size: 12px;
                    color: rgb(0,160,220);
                }
            }
            .list-content {
                padding: 0 18px;
                max-height: 217px;
                overflow: hidden;
                background-color: #fff;
                .food {
                    position: relative;
                    padding: 12px 0;
                    box-sizing: border-box;
                    border-bottom: 1px solid rgba(7,17,27,0.1);
                    .name {
                        line-height: 24px;
                        font-size: 14px;
                        color: rgb(7,17,27);
                    }
                    .price {
                        position: absolute;
                        right: 90px;
                        bottom: 12px;
                        line-height: 24px;
                        font-size: 14px;
                        font-weight: 700;
                        color: rgb(240,20,20);
                    }
                    .cartcontrol-wrapper {
                        position: absolute;
                        right: 0;
                        bottom: 6px;
                    }
                }
            }
        }

        .list-mask {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            background-color: rgba(7,17,27,0.5);
        }
    }
</style>
