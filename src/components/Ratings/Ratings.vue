<template>
    <div class="ratings" ref="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                        <star class="star" :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>
                    </div>
                     <div class="score-wrapper">
                        <span class="title">商品评分</span>
                        <star class="star" :size="36" :score="seller.foodScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                     <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
             <ratingselect 
                @select="selectRating" 
                @toggle="toggleCoutent"
                :selectType="selectType" 
                :onlyContent="onlyContent"
                :ratings="ratings"
               >
           </ratingselect>

            <div class="ratings-wrapper">
                <ul>
                    <li class="rating-item" 
                        v-show="needShow(rating.rateType,rating.text)" 
                        v-for="(rating,index) in ratings" 
                        :key="index"
                    >
                        <div class="avatar">
                            <img :src="rating.avatar" width="28" height="28">
                        </div>
                        <div class="content">
                            <h1 class="name">{{rating.username}}</h1>
                            <div class="star-wrapper">
                                <star class="star" :size="24" :score="rating.score"></star>
                                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
                                <p class="text">{{rating.text}}</p> 
                                <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                                    <span class="iconfont icon-zan21 icon"></span>
                                    <span class="item" v-for="(item,index) in rating.recommend" :key="index">{{item}}</span>
                                </div>
                                <div class="time">
                                     {{rating.rateTime | formatDate}}
                                </div>
                            </div>
                        </div>

                    </li>
                </ul>
            </div>

        </div>
    </div>
</template>

<script>
    import BScroll from 'better-scroll';

    import star from "../Star/Star.vue";
    import split from "../split/split.vue";
    import ratingselect from "../ratingselect/ratingselect.vue";

    import {formatDate} from '../../../static/date.js';


    const POSITIVE = 0; // 满意
    const NEGATIVE = 1; // 不满意
    const ALL = 2; // 所有评价

    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                ratings: [],
                showFlag: false,
                selectType: ALL,
                onlyContent: false,
            }
        },
        components: {
            star,
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


        },
        created() {
            this.$axios.post('http://result.eolinker.com/ryfVuuNe56d9619232220f51d7a2f231f3df0a6a54029bf?uri=vue-ele')
            .then((response)=> {
                console.log(response);
                this.ratings = response.data.ratings;
                this.$nextTick(()=> {
                    this.scroll = new BScroll(this.$refs.ratings,{
                        click: true,
                    })
                })
            }).catch((err)=> {
                console.log(err);
            })
        },
    }

</script>

<style scoped lang="scss">
    .ratings {
        position: absolute;
        top: 4.65rem;
        left: 0;
        bottom: 0;
        width: 100%;
        overflow: hidden;
        .overview {
            display: flex;
            padding: 0.48rem 0;
            .overview-left {
                flex: 0 0 3.65rem;
                width: 3.65rem;
                padding: 0.16rem 0;
                border-right: 1px solid rgba(7,17,27,0.1);
                text-align: center;
                @media only screen and (max-width: 320px) {
                     flex: 0 0 3.2rem;
                     width: 3.2rem;
                }
                .score {
                    margin-bottom: 0.21rem;
                    line-height: 0.75rem;
                    font-size: 24px;
                    color: rgb(255, 153, 0);
                }
                .title {
                    margin-bottom: 0.21rem;
                    line-height: 0.32rem;
                    font-size: 12px;
                    font-weight: 700;
                    color: rgba(7,17,27,1);
                }
                .rank {
                    padding-bottom: 0.16rem;
                    line-height: 0.26rem;
                    font-size: 10px;
                    color: rgb(147,153,159);
                }
            }
            .overview-right {
                flex: 1;
                padding: 0.16rem 0 0.16rem 0.64rem;
                @media only screen and (max-width: 320px) {
                     padding-left: 0.16rem;
                }
                .score-wrapper {
                    margin-bottom: 0.21rem;
                    line-height: 0.48rem;
                    font-size: 0;
                    .title {
                        display: inline-block;
                        vertical-align: top;
                        font-size: 12px;
                        color: rgb(7,17,27)
                    }
                    .star {
                        display: inline-block;
                        vertical-align: top;
                        margin: 0 0.32rem;
                    }
                    .score {
                        display: inline-block;
                        line-height: 0.48rem;
                        vertical-align: top;
                        font-size: 12px;
                        color: rgb(255,153,0);
                    }
                }
                .delivery-wrapper {
                    font-size: 0;
                    .title {
                        line-height: 0.48rem;
                        font-size: 12px;
                        color: rgb(7,17,27)
                    }
                    .delivery {
                        font-size: 12px;
                        color: rgb(147,153,159);
                        padding-left: 0.26rem;
                    }   
                }
            }
        }
        .ratings-wrapper {
            padding: 0 0.48rem;
            .rating-item {
                display: flex;
                padding: 0.48rem 0;
                border-bottom: rgba(7, 17, 27, 0.1);
                .avatar {
                    flex: 0 0 0.75rem;
                    width: 0.75rem;
                    margin-right: 0.32rem;
                    img {
                        border-radius: 50%;
                    }
                }
                .content {
                    position: relative;
                    flex: 1;
                    .name {
                        margin-bottom: 0.1;
                        line-height: 0.32rem;
                        font-size: 10px;
                        color: rgb(7, 17, 27);
                    }
                    .star-wrapper {
                        margin: 0.16rem 0;
                        font-size: 0;
                        .star {
                            display: inline-block;
                            margin-right: 0.16rem;
                            vertical-align: top;
                        }
                        .delivery {
                            display: inline-block;
                            vertical-align: top;
                            line-height: 0.32rem;
                            font-size: 10px;
                            color: rgb(147, 153, 159);
                        }
                    }
                    .text {
                        margin: 0.21rem 0;
                        line-height: 0.48rem;
                        color: rgb(7, 17, 27);
                        font-size: 12px;
                    }
                    .recommend {
                        line-height: 0.42rem;
                        font-size: 0;
                        .icon , .item {
                            display: inline-block;
                            margin: 0 8px 4px 0;
                            font-size: 9px;
                        }
                        .icon {
                            color: rgb(0, 160, 220)
                        }
                        .item {
                            padding: 0 0.16rem;
                            border: 1px solid rgba(7, 17, 27, 0.1);
                            border-radius: 1px;
                            color: rgb(147, 153, 159);
                            background: #fff;
                        }
                    }
                     .time {
                        position: absolute;
                        top: 0;
                        right: 0;
                        line-height: 0.32rem;
                        font-size: 10px;
                        color: rgb(147, 153, 159);
                     }
                }
            }
        }
    }
</style>
