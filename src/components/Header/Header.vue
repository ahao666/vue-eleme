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

            <div v-if="seller.supports" class="support-count">
                <span class="count">{{seller.supports.length}}个</span>
                <i>></i>
            </div>

        </div>

        <div class="bulletin-wrapper">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
            <i>></i>
        </div>

        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>


    </div>
</template>

<script>
    export default {
        props: {
            seller: Object
        },
        created() {
            this.classMap = ['decrease','discount','guarantee','invoice','special']
        }

    }
</script>

<style lang="scss">

    @import "../../scss/mixin.scss";

    .header {
        position: relative;
        color: #fff;
        background: rgba(7,17,27,0.4);
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
    }
</style>
