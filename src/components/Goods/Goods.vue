<template>
    <div class="goods" v-if="goods">
        <div class="menu-wrapper" ref="menuWrapper">
            <ul>
                <li class="menu-item" :class="{'current': currentIndex===index}" v-for="(item,index) in goods" :key="index" @click="selectMenu(index,$event)">
                    
                    <span class="text">
                        <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
                        {{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref="foodsWrapper">
            <ul>
                <li v-for="(item,key) in goods" :key="key" class="food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul >
                        <li v-for="(food,key) in item.foods" :key="key" class="food-item">
                            <div class="icon">
                                <img :src="food.icon" width="100%" height="100%">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span>月售{{food.sellCount}}份</span>
                                    <span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">¥{{food.price}}</span>
                                    <span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        
    </div>
</template>

<script>

    import BScroll from 'better-scroll';

    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                goods: [],
                listHeight: [], // 高度 数组
                scrollY: 0, // 跟踪 scrollY
            }
        },
        computed: {
            currentIndex() {
                for (let i = 0; i < this.listHeight.length; i++) {
                    let height1 = this.listHeight[i];
                    let height2 = this.listHeight[i + 1];
                    if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                        return i;
                    }
                }
                return 0;
            },

        },

        methods: {
            selectMenu(index,event) {
                if(event.__constructed) {
                    return;
                }
                let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook");
                let el = foodList[index];
                console.log(el);

                this.foodScroll.scrollToElement(el,300);


            },

            _initScroll() {
                this.menuScroll = new BScroll(this.$refs.menuWrapper,{
                    // BScroll 默认阻止了点击事件 所以要给它click: true
                    click: true
                });
                this.foodScroll = new BScroll(this.$refs.foodsWrapper,{
                    // 看官网！！ 有时候我们需要知道滚动的位置。当 probeType 为 1 的时候，会非实时（屏幕滑动超过一定时间后）派发scroll 事件；当 probeType 为 2 的时候，会在屏幕滑动的过程中实时的派发 scroll 事件；当 probeType 为 3 的时候，不仅在屏幕滑动的过程中，而且在 momentum 滚动动画运行过程中实时派发 scroll 事件。如果没有设置该值，其默认值为 0，即不派发 scroll 事件。
                    probeType: 3                    
                });

                // 滚动事件 BScroll提供
                this.foodScroll.on('scroll',(pos)=> {
                    // console.log(pos);  // {x: 0, y: 0}  获取滚动后的x y值
                    // 获取滚动的Y方向的值  类似window.onscroll(console.log(scroll.top));
                    this.scrollY = Math.abs(Math.round(pos.y));
                })
            },


            // 计算高度
            _calculateHeight() {

                // 获取 每个li.food-list-hook 的 dom
                let foodList = this.$refs.foodsWrapper.getElementsByClassName("food-list-hook");
                // console.log(foodList[0].offsetHeight);
                // console.log(foodList[0].clientHeight);

                var height = 0;
                this.listHeight.push(height);
                for(let i=0;i<foodList.length;i++) {
                    let item = foodList[i];
                    height += item.clientHeight;
                    this.listHeight.push(height);
                }
            }

        },

        created() {

            this.$axios.post('http://result.eolinker.com/ryfVuuNe56d9619232220f51d7a2f231f3df0a6a54029bf?uri=vue-ele')
            .then((response)=> {
                this.goods = response.data.goods;
                this.$nextTick(() => {
                    this._initScroll();
                    this._calculateHeight();
                });
            }).catch((err)=>{
                console.log(err);
            })
            

            this.classMap = ['decrease','discount','guarantee','invoice','special'];
        },

    }

</script>

<style lang="scss">

    @import "../../scss/mixin.scss";

    .goods {
        display: flex;
        position: absolute;
        top: 4.7rem;
        bottom: 1.2rem;
        width: 100%;
        overflow: hidden;
        .menu-wrapper {
            width: 2.15rem;
            flex: 0 0 2.15rem;
            background-color:#f3f5f7;
            .menu-item {
                display: table; 
                height: 1.44rem;
                width: 100%;    
                line-height: 0.48rem;
                padding: 0px 0.37rem;
                text-align: center;
                &.current {
                    position: relative;
                    z-index: 10px;
                    margin-top: -1px;
                    background: #fff;
                    font-weight: 700;
                    .text {
                        border: none;
                    }
                }
                .icon {
                    display: inline-block;
                    vertical-align: middle;
                    width: 0.32rem;
                    height: 0.32rem;
                    margin-right: 0.14rem;
                    background-size: 0.32rem 0.32rem;
                    background-repeat: no-repeat;
                    &.decrease {
                        @include bg-image('decrease_3');
                    }
                    &.discount {
                        @include bg-image('discount_3');
                    }
                    &.guarantee {
                        @include bg-image('guarantee_3');
                    }
                    &.invoice {
                        @include bg-image('invoice_3');
                    }
                    &.special {
                        @include bg-image('special_3');
                    }
                }
                .text {
                    display: table-cell;
                    width: 1.5rem;
                    vertical-align: middle;
                    border-bottom: 1px solid rgba(7,17,27,0.1);
                    font-size: 12px;
                }
            }
        }
        .foods-wrapper {
            flex: 1;
            .title {
                padding-left: 0.37rem;
                height: 0.7rem;
                line-height: 0.7rem;
                border-left: 2px solid #d9dde1;
                font-size: 12px;
                color: rgb(147,153,159);
                background-color: #f3f5f7;
            } 
            .food-item {
                display: flex;
                margin: 0.48rem;
                padding-bottom: 0.48rem;
                border-bottom: 1px solid rgba(7,17,27,0.1);
                &:last-child {
                    border: none;
                    padding-bottom: 0;
                }
                .icon {
                    flex: 0 0 57px;
                    margin-right: 0.38rem;
                }
                .content {
                    flex: 1;
                    .name {
                        margin: 2px 0 8px 0;
                        height: 0.38rem;
                        line-height: 0.38rem;
                        font-size: 14px;
                        color: #000;
                        font-weight: 700;
                    }
                    .desc,.extra  {
                        line-height: 0.3rem;
                        font-size: 11px;
                        color: rgba(147,153,159,1);
                    }
                    .desc {
                        margin-bottom: 0.21rem;
                    }
                    .extra {
                        .count {
                            margin-right: 0.32rem;
                        }
                    }
                    .price {
                        font-weight: 700;
                        line-height: 0.64rem;
                        .now {
                            margin-right: 0.21rem;
                            font-size: 14px;
                            color: rgb(240,20,20);
                        }
                        .old {
                            text-decoration: line-through;
                            font-size: 10px;
                            color: rgba(147,153,159,1);
                        }
                    }
                    
                }
            }
        }
    }
</style>
