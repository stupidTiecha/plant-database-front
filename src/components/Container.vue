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
            // Automatically adjust the size of the window elements (left side bar and right side details)
            // Wait for 50 milliseconds because after clicking the left column, the execution is too fast and the correct size cannot be obtained
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
            //Perform operation after waiting for x ms
            sleep : function (ms) {
                return new Promise(resolve =>
                    setTimeout(resolve, ms));
            }
        }
        ,
        mounted() {
            let that = this;
            //Because the height cannot be obtained correctly when the zoom button is executed, resize is performed once per second
            window.setInterval(this.resize,1000);
            //Automatically adjust when the window size changes
            window.onresize = () => {
                that.resize();
            };
            //When the window scrolls, adjust the login position in the upper right corner
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