<template>
    <div v-bind:id="container" v-bind:style="{height : containerHeight}">
        <left-banner></left-banner>
        <detail-window v-bind:id="detailWindow" v-bind:style="{width : detailWidth}" ref="detail"></detail-window>
    </div>
</template>

<script>
    import LeftBanner from "./LeftBanner";
    import DetailWindow from "./DetailWindow";

    export default {
        name: "Container",
        components: {DetailWindow, LeftBanner},
        data () {
            return {
                container : 'container',
                detailWindow : 'detailWindow',
                containerHeight : ((window.innerHeight - 140) <= 400 ? 400 : (window.innerHeight - 140)) + 'px',
                detailWidth : ((window.innerWidth - 217) <= 500 ? 500 :(window.innerWidth - 200)) + 'px',
            }
        },
        methods : {
            //自动调整窗口元素大小（左侧栏和右侧详情）
            // 等待50毫秒是因为点击左侧栏后，执行过快，无法获取到正确的size
            resize : function () {
                this.sleep(50).then(() => {
                    let childEl = this.$refs.detail.$refs.childWindow.$el;
                    let childHeight = childEl.clientHeight;
                    let el = this.$el;
                    let height = window.innerHeight - 140;
                    let width = window.innerWidth - 200;
                    let autoHeight = height <= 400 ? 400 : height;
                    let autoWidth = width <= 500 ? 500 : width;
                    // console.log(childHeight,autoHeight);
                    if (childHeight >= autoHeight) {
                        autoHeight = childHeight ;
                    }
                    el.setAttribute('style','height : ' + autoHeight + 'px');
                    el.lastElementChild.setAttribute('style', 'width : ' + autoWidth + 'px');
                    childEl.setAttribute('style', 'width : ' + autoWidth + 'px');
                })

            },
            //等待x ms 后执行操作
            sleep : function (ms) {
                return new Promise(resolve =>
                    setTimeout(resolve, ms));
            }
        }
        ,
        mounted() {
            let that = this;
            //因为放大按钮执行时无法正确获取height，每秒执行一次resize
            window.setInterval(this.resize,1000);
            //窗口size发生变化时，自动调整
            window.onresize = () => {
                that.resize();
            };

        },
    }
</script>

<style scoped>
#container {
    top: 140px;
    height: 100%;
    width: 100%;
    position: absolute;
    display:inline-block;
    overflow-x: hidden;
    overflow-y: hidden;
}
#detailWindow{
    height: 100%;
    left: 200px;
    background-color: whitesmoke;
    position: relative ;
    text-align: center;
}
</style>