<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar">
                <img :src="seller.avatar" width="64" height="64">
            </div>
            <div class="cotent">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}} / {{seller.deliveryTime}} 分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>                
            </div>

            <div v-if="seller.supports" class="support-count" @click="showDetail">
                <span class="count">{{seller.supports.length}}个</span>
                <i>></i>
            </div>

        </div>

        <div class="bulletin-wrapper" @click="showDetail">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
            <i>></i>
        </div>

        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>

        <transition name="fade">
        <div v-show="detailShow" class="detail">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <Star :size="48" :score="seller.score"></Star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class="supports">
                        <li class="support-item" v-for="(item,key) in seller.supports" :key="key">
                            <span class="icon" :class="classMap[seller.supports[key].type]"></span>
                            <span class="text">{{seller.supports[key].description}}</span>
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

            <div class="detail-close" @click="hideDetail">X</div>
        </div>
        </transition>

    </div>
</template>

<script>
    import Star from '../Star/Star.vue';

    export default {
        props: {
            seller: Object
        },
        components: {
            Star
        },
        data() {
            return {
                detailShow: false
            }
        },
        methods: {
            showDetail() {
                this.detailShow = true;
            },
            hideDetail() {
                this.detailShow = false;
            }
        },
        created() {
            this.classMap = ['decrease','discount','guarantee','invoice','special']
        }

    }
</script>

<style lang="scss">

    @import "../../scss/mixin.scss";
    @import "../../scss//base.scss";

    .fade-enter-active,.fade-leave-active {
        background: rgba(7,17,27,0.8);
        transition: all 0.5s linear;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        background: rgba(7,17,27,0);        
        opacity: 0;
    }

    .header {
        position: relative;
        color: #fff;
        background: rgba(7,17,27,0.4);
        overflow: hidden;
        .content-wrapper {
            position: relative;
            padding: 0.64rem 0.32rem 0.48rem 0.64rem;
            font-size: 0;
            .avatar {
                display: inline-block;
                img {
                    border-radius: 2px;
                }
            }
            .cotent {
                display: inline-block;
                font-size: 14px;
                margin-left: 0.36rem;
                .title {
                    margin: 0.05rem 0 0.2rem 0;
                    .brand {
                        display: inline-block;
                        width: 0.8rem;
                        height: 0.48rem;
                        @include bg-image('brand');
                        background-size: 30px 18px;
                        background-repeat: no-repeat;
                        vertical-align: top;
                    }
                    .name {
                        margin-left: 0.16rem;
                        font-size: 16px;
                        line-height: 0.48rem;
                        font-weight: 500;
                    }
                }
                .description {
                    margin-bottom: 0.27rem;
                    line-height: 0.32rem;
                    font-size: 12px;
                }
                .support {
                    .icon {
                        display: inline-block;
                        vertical-align: top;
                        width: 0.32rem;
                        height: 0.32rem;
                        margin-right: 0.27rem;
                        background-size: 0.32rem;
                        background-repeat: no-repeat;
                        &.decrease {
                            @include bg-image('decrease_1');
                        }
                        &.discount {
                            @include bg-image('discount_1');
                        }
                        &.guarantee {
                            @include bg-image('guarantee_1');
                        }
                        &.invoice {
                            @include bg-image('invoice_1');
                        }
                        &.special {
                            @include bg-image('special_1');
                        }
                    }
                    .text {
                        vertical-align: top;
                        line-height: 0.32rem;
                        font-size: 12px;
                    }
                }
            }
            .support-count {
                position: absolute;
                right: 0.32rem;
                bottom: 0.48rem;
                padding: 0 0.22rem;
                width: 1.2rem;
                height: 0.64rem;
                line-height: 0.64rem;
                border-radius: 14px;
                background: rgba(0,0,0,0.2);
                .count {
                    font-size: 10px;
                }
                i {
                    font-size: 10px;
                    font-style: normal;
                    margin-left: 0.1rem;
                }
            }
        }

        .bulletin-wrapper {
            position: relative;
            height: 0.75rem;
            line-height: 0.75rem;
            padding: 0 0.58rem 0 0.21rem;
            text-overflow: ellipsis;
            overflow: hidden;
            white-space: nowrap;
            background-color: rgba(7,17,27,0.2);
            .bulletin-title {
                display: inline-block;
                vertical-align: top;
                margin-top: 0.22rem;
                width: 22px;
                height: 12px;
                @include bg-image('bulletin');
                background-size: 22px 12px;
                background-repeat: no-repeat;
            }
            .bulletin-text {
                vertical-align: top;
                font-size: 10px;
                margin: 0 0.11rem;
            }
            i {
                position: absolute;
                bottom: 0.025rem;
                right: 0.125rem;
                font-size: 12px;
                font-style: normal;
            }
        }
        
        .background {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            filter: blur(10px);
        }

        .detail {
            position: fixed;
            top: 0;
            left: 0;
            z-index: 9999;
            width: 100%;
            height: 100%;
            overflow: auto;
            background: rgba(7,17,27,0.8);
            .detail-wrapper {
                min-height: 100%;
                .detail-main {
                    margin-top: 1.7rem;
                    padding-bottom: 1.7rem;
                    .name {
                        line-height: 16px;
                        text-align: center;
                        font-size: 16px;
                        font-weight: 700;
                    }
                    .star-wrapper {
                        margin-top: 0.48rem;
                        padding: 0.05rem 0;
                        text-align: center;
                    }
                    .title {
                        display: flex;
                        width: 80%;
                        margin: 0.8rem auto 0.64rem auto;
                        .line {
                            flex: 1;
                            position: relative;
                            top: -0.21rem;
                            border-bottom: 1px solid rgba(255,255,255,0.2);
                        }
                        .text {
                            padding: 0 0.32rem;
                            font-size: 14px;
                        }
                    }
                    .supports {
                        width: 80%;
                        margin: 0 auto;
                        .support-item {
                            padding: 0 0.32rem;
                            margin-bottom: 0.32rem;
                            font-size: 0;
                            &:last-child {
                                margin-bottom: 0;
                            }
                            .icon {
                                display: inline-block;
                                width: 0.42rem;
                                height: 0.42rem;
                                vertical-align: middle;
                                margin-right: 0.16rem;
                                background-size:  0.42rem 0.42rem;
                                background-repeat: no-repeat;
                                &.decrease {
                                    @include bg-image('decrease_1');
                                }
                                &.discount {
                                    @include bg-image('discount_1');
                                }
                                &.guarantee {
                                    @include bg-image('guarantee_1');
                                }
                                &.invoice {
                                    @include bg-image('invoice_1');
                                }
                                &.special {
                                    @include bg-image('special_1');
                                }
                            }
                            .text {
                                line-height: 12px;
                                font-size: 12px;
                                vertical-align: middle
                            }
                        }
                    }
                    .bulletin {
                        width: 80%;
                        margin: 0 auto;
                        .content {
                            padding: 0 0.32rem;
                            line-height: 0.64rem;
                        }
                    }

                }
            }
            .detail-close {
                position: relative;
                width: 0.85rem;
                height: 0.85rem;
                margin: -1.7rem auto 0 auto;
                clear: both;
                font-size: 32px;
            }
        }


    }
</style>
