<template>
    <div v-bind:id="leftBar">
        <div v-bind:id ="menuList">
            <li v-for="(menu,slot) in menus" :key="slot" v-on:click="trigger(menu.path,menu.url)" >
                <a href="#">{{menu.description}}</a><b-icon :icon="menu.icon" style="float: right ; margin-right: 5px ;margin-top: 9px"></b-icon>
            </li>
        </div>
        <div id="contactUs">
            <p><span style="font-weight: bold">Phone :</span> +27 21 461 9265<br/>
                <span style="font-weight: bold">Email : </span><span style="color: darkolivegreen">info@greenpop.org</span><br/>
                <span style="font-weight: bold">Business Hours: </span> 8:00am - 5:00pm, Monday to Friday </p>
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
                //The left menu option, add only need to write according to the format, path refers to the component that is
                // switched after clicking (need to write the corresponding component), the icon uses bootstrapVue
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
                        //ContactUs external links
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
    background-color: #3b3a4a;
    height: 100%;
    width: 200px;
    position: absolute;
    overflow: hidden;
}
#menuList {
    /*top: 50%;*/
    height: 100%;
    /*float: top;*/
    overflow: hidden;
    position: relative;
    text-align: center;
}
/*#menuList li a{*/
/*    float: left;*/
/*}*/

#menuList li a:link,a:hover,a:visited,a:active {
    color: #f2f2f4;
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
    background-color: #1ebad6;
}
#contactUs {
    color: #f2f2f4;
    position: absolute;
    width: 200px;
    height: 100px;
    z-index: 999;
    line-height: 25px;

}
</style>