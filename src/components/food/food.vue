<template>
    <transition name="move">
        <div class="food" ref="food" v-show="showFlag">
            <div class="food-content">
                <div class="image-header">
                    <img :src="food.image" >
                    <div class="back" @click="hide">
                        <i class="iconfont icon-right"></i>
                    </div>
                </div>

                <div class="content">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="detail">
                        <span class="seller-count">月售{{food.sellCount}}份</span>
                        <span class="rating">好评率{{food.rating}}</span>
                    </div>
                    <div class="price">
                        <span class="now">¥{{food.price}}</span>
                        <span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                    </div>

                     <div class="cartcontrol-wrapper">
                        <cartcontrol :food="food"></cartcontrol>
                    </div>

                    <transition name="fade">
                        <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
                    </transition>

                </div>

                <split v-show="food.info"></split>
                <div class="info" v-show="food.info">
                    <h1 class="title">商品信息</h1>
                    <p class="text" >{{food.info}}</p>
                </div>
                <split></split>

                <!-- 商品评价 -->
                <div class="rating">
                    <h1 class="title">商品评价</h1>
                    <ratingselect 
                        @select="selectRating" 
                        @toggle="toggleCoutent"
                        :ratings="food.ratings" 
                        :selectType="selectType" 
                        :onlyContent="onlyContent" 
                        :desc="desc"
                    >
                    </ratingselect>

                    <div class="rating-wrapper">
                        <ul v-show="food.ratings && food.ratings.length">
                            <li v-show="needShow(rating.rateType,rating.text)" class="rating-item" v-for="(rating,index) in food.ratings" :key="index">
                                <div class="user">
                                    <span class="name">{{rating.username}}</span>
                                    <img class="avatar" :src="rating.avatar" width="12" height="12" alt="">
                                </div>
                                <div class="time">
                                    {{rating.rateTime | formatDate}}
                                </div>
                                <p class="text">
                                    <span :class="{
                                        'iconfont icon-zan21 icon1': rating.rateType === 0,
                                        'iconfont icon-zan2 icon2': rating.rateType === 1
                                    }"></span>{{rating.text}}
                                </p>
                            </li>
                        </ul>
                        <div class="no-rating" v-show="!food.ratings || !food.ratings.length"></div>

                    </div>
                </div>
               
            </div>
        </div>
    </transition>
</template>

<script>
    import Vue from 'vue';
    import BScroll from 'better-scroll';
    import cartcontrol from '../cartcontrol/cartcontrol.vue';
    import split from '../split/split.vue';
    import ratingselect from '../ratingselect/ratingselect.vue';

    import {formatDate} from '../../../static/date.js';

    const POSITIVE = 0; // 满意
    const NEGATIVE = 1; // 不满意
    const ALL = 2; // 所有评价

    export default {
        props: {
            food: {
                type: Object
            }
        },
        data() {
            return {
                showFlag: false,
                selectType: ALL,
                onlyContent: false,
                desc: {
                    all: "全部",
                    positive: '推荐',
                    negative: '吐槽'
                }
            }
        },
        components: {
            cartcontrol,
            split,
            ratingselect
        },
        filters: {
            formatDate(time) {
                let date = new Date(time);
                return formatDate(date,'yyyy-MM-dd hh:mm');
            }
        },
        methods: {
            show() {
                this.showFlag = true;

                // 初始化ratingselect组件的数据
                this.selectType = ALL;
                this.onlyContent = true;

                this.$nextTick(()=> {
                    if(!this.scroll) {
                        this.scroll = new BScroll(this.$refs.food,{
                            click: true,
                        })
                    }else {
                        this.scroll.refresh();
                    }
                })
            },

            // 更改 ratingselect组件中 商品评价的 type;
            selectRating(type) {
                this.selectType = type;
                this.$nextTick(()=> {
                    this.scroll.refresh();
                })
            },
            // 更改 ratingselect组件中 商品评价的 onlyContent;
            toggleCoutent() {
                this.onlyContent = !this.onlyContent;
                this.$nextTick(() => {
                    this.scroll.refresh();
                });
            },


            hide() {
                this.showFlag = false;
            },
            addFirst(event) {
                if(!event._constructed) {
                    return;
                }
                // 派发事件
                this.$emit('add',event.target);
                
                Vue.set(this.food,'count',1);

            },

            // 商品评价内容展示
            needShow(type,text) {
                if (this.onlyContent && !text) {
                    return false;
                }
                if (this.selectType === ALL) {
                    return true;
                }else {
                    return type === this.selectType;
                }

            }
        },
        created () {
            console.log(this.food);
        }
    }
</script>

<style scoped lang="scss">

    .move-enter-active,.move-leave-active {
        transform: translate3d(0,0,0);
        transition: all 0.2s linear;
    }
    .move-enter, .move-leave-to /* .fade-leave-active below version 2.1.8 */ {
        transform: translate3d(100%,0,0);
    }

    .fade-enter-active,.fade-leave-active {
        opacity: 1;
        transition: all 0.2s linear;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0;
    }

    .food {
        position: fixed;
        top: 0;
        left: 0;
        bottom: 48px;
        z-index: 998;
        width: 100%;
        background-color: #fff;
        .image-header {
            position: relative;
            width: 100%;
            height: 0;
            padding-top: 100%;
            img {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
            }
            .back {
                position: absolute;
                top: 10px;
                left: 0;
                i { 
                    font-style: normal;
                    display: block;
                    padding: 10px;
                    font-size: 20px;
                    color: #fff;
                }
            }
        }
        .content {
            position: relative;
            padding: 18px;
            .title {
                line-height: 14px;
                margin-bottom: 8px;
                font-size: 14px;
                font-weight: 700;
                color: rgb(7, 17, 27);
            }
            .detail {
                margin-bottom: 18px;
                line-height: 10px;
                font-size: 0;
                .seller-count,.rating {
                    font-size: 12px;
                    color: rgb(147,153,159);
                }
                .seller-count {
                    margin-right: 12px;
                }
            }
            .price {
                font-weight: 700;
                line-height: 24px;
                .now {
                    margin-right: 8px;
                    font-size: 14px;
                    color: rgb(240,20,20);
                }
                .old {
                    text-decoration: line-through;
                    font-size: 10px;
                    color: rgb(147,153,159);
                }
            }
            .cartcontrol-wrapper {
            position: absolute;
            right: 12px;
            bottom: 12px;
            }
            .buy {
                position: absolute;
                right: 18px;
                bottom: 18px;
                z-index: 10;
                height: 24px;
                line-height: 24px;
                padding: 0 12px;
                box-sizing: border-box;
                border-radius: 18px;
                font-size: 10px;
                color: #fff;
                background-color: rgb(0,160,220);
            }
        }
        .info {
            padding: 0.48rem;
            .title {
                line-height: 14px;
                margin-bottom: 0.16rem;
                font-size: 14px;
                font-weight: 700;
                color: rgb(7,17,27);
            }
            .text {
                line-height: 24px;
                padding: 0 0.21rem;
                color: rgb(77,85,93);
                font-size: 12px;
            }
        }
        .rating {
             padding-top: 18px;
             .title {
                 line-height: 14px;
                 margin-left: 18px;
                 font-size: 14px;
                 color: rgb(7,17,27);
             }
             .rating-wrapper {
                 padding: 0 18px;
                 .rating-item {
                     position: relative;
                     padding: 16px 0;
                     border-bottom: 1px solid rgba(7,17,27,0.1);
                     .user {
                         position: absolute;
                         right: 0;
                         top: 16px;
                         line-height: 12px;
                         font-size: 0;
                         .name {
                             display: inline-block;
                             margin-right: 6px;
                             vertical-align: top;
                             font-size: 10px;
                             color: rgb(147,153,159);
                         }
                         .avatar {
                             border-radius: 50%;
                         }
                     }
                     .time {
                         margin-bottom: 6px;
                         line-height: 12px;
                         font-size: 10px;
                         color: rgb(147,153,159);
                     }
                     .text {
                         line-height: 16px;
                         font-size: 12px;
                         color: rgb(7,17,27);
                         .icon1,.icon2 {
                             margin-right: 4px;
                             line-height: 16px;
                         }
                         .icon1 {
                             color: rgb(0,160,220);
                         }
                         .icon2 {
                             color: rgb(147,153,159);
                         }
                     }
                 }
                 .no-rating {
                     padding: 16px 0;
                     font-size: 12px;
                     color: rgb(147,153,159);
                 }
             }
        }
        
    }
</style>
