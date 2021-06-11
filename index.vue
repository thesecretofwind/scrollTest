<template>
    <div :style="`height:${viewHeight}px;overflow-y:scroll`" @scroll="handleScroll" class="out">
        <div :style="`height:${scrollHeight}px`" class="list">
            <div class="item_box" :style="`transform:translateY(${offsetY}px)`">
                <div class="item"
                     :style="`height: ${itemHeight}px`"
                     v-for="(item, index) of list"
                     :key="index">
                    {{ item }}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "List",
        props: {
            data: Array,   // 列表总数据
            viewHeight: Number, // 外部高度
            itemHeight: Number, // 单项高度
        },
        data() {
            return {
                scrollHeight: '', // 整个滚动列表高度
                list: [],    // 每次显示的数据
                showNum: '',
                offsetY: '',// 动态偏移量- 外层的盒子进行滚动设置
                previousTime: '',
                nowTime:''
            }
        },
        mounted() {
            this.scrollHeight = this.data.length * this.itemHeight; //计算列表list的滚动条高度
            this.showNum = Math.floor(this.viewHeight / this.itemHeight); //计算在展示视口中展示的列表数目
            this.list = this.data.slice(0, this.showNum); // 截取数目
            this.lastTime = new Date().getTime();   //计算时间，用于节流
        },
        methods: {
            handleScroll(e) {
                this.nowTime = new Date().getTime()
                if ( this.nowTime - this.previousTime >= 16) {
                    let scrollTop = e.target.scrollTop; // 获取滚动条滚动的距离
                    
                    this.offsetY = scrollTop - (scrollTop % this.itemHeight);   //根据滚动的距离计算滚动了多少列表项
                    this.list = this.data.slice(     // 根据滚动过去的滚动条来设置视口显示的数据
                        Math.floor(scrollTop / this.itemHeight), 
                        Math.floor(scrollTop / this.itemHeight) + this.showNum
                    )
                    this.previousTime = this.nowTime;   //更新节流函数
                }
            }
        }
    }
</script>

<style scoped>

</style>