<template>
    <div v-bind:id="container" v-bind:style="{height : containerHeight}">
<!--        <left-banner ref="leftBanner"></left-banner>-->
        <detail-window v-bind:id="detailWindow" v-bind:style="{width : detailWidth}" ref="detail"></detail-window>
    </div>
</template>

<script>
    import LeftBanner from "./LeftBanner";
    import DetailWindow from "./DetailWindow";

    export default {
        name: "Container",
        components: {DetailWindow},
        data () {
            return {
                container : 'container',
                detailWindow : 'detailWindow',
                containerHeight : ((window.innerHeight - 140) <= 400 ? 400 : (window.innerHeight - 140)) + 'px',
                detailWidth : (window.innerWidth  <= 500 ? 500 :window.innerWidth) + 'px',
            }
        },
        methods : {
            //自动调整窗口元素大小（左侧栏和右侧详情）
            // 等待50毫秒是因为点击左侧栏后，执行过快，无法获取到正确的size
            resize : function () {
                this.sleep(50).then(() => {
                    let detail = this.$refs.detail.$refs.childWindow.$el;
                    let searchForm = detail.firstElementChild;
                    let detailHeight = detail.clientHeight;
                    let el = this.$el;
                    let height = window.innerHeight - 140;
                    let width = window.innerWidth;
                    let autoHeight = height <= 550 ? 550 : height;
                    let autoWidth = width <= 700 ? 700 : width;
                    // console.log(detailHeight,autoHeight);
                    if (detailHeight >= autoHeight) {
                        autoHeight = detailHeight ;
                    }
                    if (searchForm !== null && searchForm.clientHeight !== 0) {
                        let clientHeight = searchForm.clientHeight;
                        autoHeight = clientHeight + 100 ;
                        // console.log(clientHeight,autoHeight,detailHeight);
                    }
                    el.setAttribute('style','height : ' + autoHeight + 'px');
                    el.lastElementChild.setAttribute('style', 'width : ' + autoWidth + 'px');
                    detail.setAttribute('style', 'width : ' + autoWidth + 'px');
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
            window.onscroll = () => {
                let scroll = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
                let temp = this.$parent.$refs.top.$el.lastChild;
                let top = 140 - scroll >= 0 ? 140 -scroll : 0;
                temp.setAttribute("style","top : " + top + 'px');
            }
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
    /*left: 200px;*/
    background-color: white;
    position: relative ;
    text-align: center;
}
</style>