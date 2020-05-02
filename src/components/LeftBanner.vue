<template>
    <div v-bind:id="leftBar">
        <div v-bind:id ="menuList">
            <li v-for="(menu,slot) in menus" :key="slot" v-on:click="trigger(menu.path,menu.url)" >
                <a href="#">{{menu.description}}</a><b-icon :icon="menu.icon" style="float: right ; margin-right: 5px ;margin-top: 9px"></b-icon>
            </li>
        </div>
    </div>
</template>

<script>

    export default {
        name: "LeftBanner",
        data() {
                return {
                leftBar : 'leftBar',
                menuList : 'menuList',
                //左侧菜单选项，添加只需要按格式写好，path指点击后切换的组件(需要写好对应的组件)，icon用的是bootstrapVue的
                menus : {
                    home : {
                        description : 'Home',
                        path : 'homePage',
                        icon : 'house',
                    },
                    search : {
                        description : 'Search',
                        path : 'Search',
                        icon : 'search',
                    },
                    aboutTheDatabase : {
                        description : 'About The Database',
                        path : 'AboutTheDatabase',
                        icon : 'server',
                    },
                    formatAndTips : {
                        description : 'Format And Tips',
                        path : 'FormatAndTips',
                        icon : 'star-fill',
                    },
                    contactUs : {
                        description : 'Contact Us',
                        path : 'homePage',
                        icon : 'people-fill',
                        //ContactUs 的外部链接
                        url : 'http://www.landcareresearch.co.nz/about/contact/enquiry_form.asp?recipient_name=Maori%20Plant%20Use&recipient_imera=NgaTipuWhakaoranga',
                    },

                }
            }
        },
        methods : {
            trigger : function (str,url) {
                if (url !== '' && url !== undefined) {
                    window.open(url) ;
                    return;
                }
                let $parent = this.$parent;
                $parent.$refs.detail.currentComponent = str;
                this.menus.contactUs.path = str;
                $parent.resize();
            }

        },
        mounted() {
        },
    }


</script>

<style scoped>
#leftBar {
    background-color: #feffdb;
    height: 100%;
    width: 200px;
    position: absolute;
    overflow: hidden;
}
#menuList {
    top: 60px;
    height: 100%;
    float: top;
    overflow: hidden;
    position: relative;
}
#menuList li a{
    float: left;
}

#menuList li a:link,a:hover,a:visited,a:active {
    color: #9f9072;
    font-weight: bold ;
    text-decoration: none;
}
#menuList li:hover{
    cursor:pointer
}
#menuList li {
    height: 30px;
    line-height: 30px;
    width: 100%;
    float: left;
    /*font-size: large;*/
    border-bottom-style: solid;
    border-bottom-width: 1px;
    border-bottom-color: black;
    list-style-type: none;
}
#menuList li:hover {
    background-color: #c8cbc6;
}
</style>