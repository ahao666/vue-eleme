<template>
    <div class="ratingselect">
        <div class="rating-type">
            <span @click="select(2,$event)" class="block positive" :class="{'active': selectType === 2}">{{desc.all}}
                <span class="count">{{ratings.length}}</span>
            </span>
            <span @click="select(0,$event)" class="block positive" :class="{'active': selectType === 0}" >{{desc.positive}}
                <span class="count">{{positives.length}}</span>
            </span>
            <span @click="select(1,$event)" class="block negative" :class="{'active': selectType === 1}">{{desc.negative}}
                <span class="count">{{negatives.length}}</span>
            </span>
        </div>

        <div @click="toggleCoutent" class="switch" :class="{'on': onlyContent === true}">
            <span class="iconfont icon-ok icon"></span>
            <span class="text">只看有内容的评价</span>
        </div>

        
    </div>
</template>

<script>

    const POSITIVE = 0; // 满意
    const NEGATIVE = 1; // 不满意
    const ALL = 2; // 所有评价

    export default {
        props: {
            // 评价内容
            ratings: {
                type: Array,
                default() {
                    return [];
                }
            },
            // 选择类型
            selectType: {
                type: Number,
                default: ALL
            },
            // 只看有内容的评价
            onlyContent: {
                type: Boolean,
                default: false 
            },
            // 描述
            desc: {
                type: Object,
                default() {
                    return {
                        all: '全部',
                        positive: '满意',
                        negative: '不满意'
                    }
                }
            }
        },
        computed: {
            positives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE;
                })
            },
            negatives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE;
                });
            }
        },
        methods: {
            select(type,event) {
                if(!event._constructed) {
                    return;
                }
                this.$emit("select",type);
            },
            toggleCoutent(event) {
                 if(!event._constructed) {
                    return;
                }
                this.$emit('toggle');
            }
        },
        created () {
            console.log(this.ratings);
        }
    }
</script>

<style scoped lang="scss">
    .ratingselect {
        .rating-type {
            padding: 18px 0;
            margin: 0 18px;
            border-bottom: 1px solid rgba(7,17,27,0.1);
            font-size: 0;
            .block {
                display: inline-block;
                padding: 8px 12px;
                margin-right: 8px;
                border-radius: 1px;
                font-size: 12px;
                color: rbg(77,85,93);
                .count {
                    font-size: 8px;
                    margin-left: 2px;
                    line-height: 16px;
                }
                &.active {
                    color: #fff;
                }
                &.positive {
                    background: rgba(0,160,220,0.2);
                    &.active {
                        background: rgb(0,160,220);
                    }
                }
                &.negative {
                    background: rgba(77,85,93,0.2);
                    &.active {
                        background: rgb(77,85,93);
                    }
                }
            }   
        }
        .switch {
            padding: 12px 18px;
            line-height: 24px;
            border-bottom: 1px solid rgba(7,17,27,0.1);
            color: rgb(147,153,159);
            font-size: 0;
            &.on {
                .icon {
                    color: #00c850;
                }
            }
            .icon {
                display: inline-block;
                vertical-align: top;
                margin-right: 6px;
                font-size: 24px;
            }
            .text {
                display: inline-block;
                font-size: 12px;
            }
        }
    }
</style>
