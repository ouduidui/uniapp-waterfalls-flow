<template>
    <view class="wf-page">
        <!--    left    -->
        <view>
            <view id="left" v-if="leftList.length">
                <view v-for="(item,index) in leftList" :key="index"
                      class="wf-item" @tap="itemTap(item)">
                    <WaterfallsFlowItem :item="item"/>
                </view>
            </view>
        </view>


        <!--    right    -->
        <view>
            <view id="right" v-if="rightList.length">
                <view v-for="(item,index) in rightList" :key="index"
                      class="wf-item" @tap="itemTap(item)">
                    <WaterfallsFlowItem :item="item"/>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
import WaterfallsFlowItem from '../WaterfallsFlowItem/WaterfallsFlowItem.vue'

export default {
    components: {
        WaterfallsFlowItem
    },
    props: {
        // 瀑布流列表
        wfList: {
            type: Array,
            require: true
        },
        updateNum: {
            type: Number,
            default: 10
        }
    },
    data() {
        return {
            allList: [],       // 全部列表
            leftList: [],      // 左边列表
            rightList: [],     // 右边列表
            mark: 0,           // 列表标记
            boxHeight: [],     // 下标0和1分别为左列和右列高度
        };
    },
    watch: {
        // 监听列表数据变化
        wfList() {
            // 如果数据为空或新的列表数据少于旧的列表数据（通常为下拉刷新或切换排序或使用筛选器），初始化变量
            if (!this.wfList.length ||
                (this.wfList.length === this.updateNum && this.wfList.length <= this.allList.length)) {
                this.allList = [];
                this.leftList = [];
                this.rightList = [];
                this.boxHeight = [];
                this.mark = 0;
            }

            // 如果列表有值，调用waterfall方法
            if (this.wfList.length) {
                this.allList = this.wfList;
                this.waterFall()
            }
        },

        // 监听标记，当标记发生变化，则执行下一个item排序
        mark() {
            const len = this.allList.length;
            if (this.mark < len && this.mark !== 0) {
                this.waterFall();
            }
        }
    },
    methods: {
        // 瀑布流排序
        waterFall() {
            const i = this.mark;
            if (i === 0) {
                // 初始化，从左边开始插入
                this.leftList.push(this.allList[i]);
                // 更新左边列表高度
                this.getViewHeight(0);
            } else if (i === 1) {
                // 第二个item插入，默认为右边插入
                this.rightList.push(this.allList[i]);
                // 更新右边列表高度
                this.getViewHeight(1);
            } else {
                // 根据左右列表高度判断下一个item应该插入哪边
                const leftOrRight = this.boxHeight[0] > this.boxHeight[1] ? 1 : 0;
                if (leftOrRight) {
                    this.rightList.push(this.allList[i])
                } else {
                    this.leftList.push(this.allList[i])
                }
                // 更新插入列表高度
                this.getViewHeight(leftOrRight);
            }
        },
        // 获取列表高度
        getViewHeight(leftOrRight) {
            const query = uni.createSelectorQuery().in(this);
            const id = leftOrRight ? '#right' : '#left';
            // 使用nextTick，确保页面更新结束后，再请求高度
            this.$nextTick(() => {
                query.select(id).boundingClientRect(res => {
                    res ? this.boxHeight[leftOrRight] = res.height : '';
                    this.mark = this.mark + 1;
                }).exec();
            })
        },

        // item点击
        itemTap(item) {
            this.$emit('itemTap', item)
        }
    }
}
</script>

<style lang="scss" scoped>
$page-padding: 10px;
$grid-gap: 10px;

.wf-page {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: $grid-gap;
    padding: 10px $page-padding;
}

.wf-item {
    width: calc((100vw - 2 * #{$page-padding} - #{$grid-gap}) / 2);
    padding-bottom: $grid-gap;
}
</style>
