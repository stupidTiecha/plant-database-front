<template>
    <div v-bind:id="topBar" >
        <div v-bind:id="upperBar" >
            <div v-bind:id="logo">
                <div v-bind:id="logoPic">
                    <a href="#"><img :src="logoPath" alt="这里是logo图片"></a>
                </div>

            </div>
            <div v-bind:id="logoDescription">
                <p style="display: inline-block ;
                    background-color: rgba(166,171,167,1);
                    height: 30px;
                    font-size: larger;
                    line-height: 30px;
                    margin-left: 8px ;
                    min-width: 176px;
                    color: black;
                    font-family: 'Times New Roman',serif;
                    text-align: center">&nbsp;{{description}}&nbsp;</p>
            </div>
            <div v-bind:id="floatPic" v-for="(path,index) in floatPics  " :key ='index'>
                <img :src="path" :alt="index + '号图片'"/>
            </div>
        </div>
        <div v-bind:id="bottomBar" >
            <div style="display: inline-block">
                <a v-for="(select,key,slot) in selectBars" :key="key" href="#" v-on:click="goOut(select.href)">
                    <span v-if="slot !== 0"> | </span>{{select.value}}
                </a>
            </div>
        </div>
        <div id="floatBar">
            <a href ="#" v-on:click ="login" style=" line-height: 25px;">{{user.userName === '' ? 'Login' : 'Hello ' + user.userName}}</a>
            <span> | </span>
            <a href="#"
               v-on:click="goOut('https://www.facebook.com/landcareresearch')"
               style="margin-right: 2px"><img src="../../public/facebook.jpg" height="20" width="20"/></a>
            <a href="#"v-on:click="goOut('https://twitter.com/mwlr_nz')" ><img src="../../public/twitter.jpg" height="20" width="20"/></a>
        </div>
    </div>
</template>

<script>

    export default {
        name: "HomePage",
        data() {
            return {
                topBar : 'default',
                logo : 'logo',
                floatPic : 'floatPic',
                upperBar : 'upperBar',
                bottomBar : 'bottomBar',
                logoPic : 'logoPic',
                logoDescription : 'logoDescription',
                //Here is the logo image path
                logoPath : require('../../public/logo-1.png'),
                //Here is the introduction under the logo
                description : 'Māori Plant Use',
                //Here is the picture address of the floating head, you can add multiple
                floatPics : [require('../../public/plant001.jpg'),require('../../public/plant002.jpg'),require('../../public/plant003.jpg')],
                //Here the horizontal bar is selected, according to the format when adding
                selectBars : {

                    Home : {
                        href : "homePage",
                        value : "Home"
                    },
                    Search : {
                        href : "search",
                        value : "Search"
                    }
                    ,
                    Database : {
                        //External link address
                        href : 'http://www.landcareresearch.co.nz/databases/index.asp',
                        value : "Database"
                    },
                    Plant : {
                        href : "http://www.landcareresearch.co.nz/databases/db_category.asp?DB_Coll_Category_ID=2",
                        value : "Plant"
                    },
                    AboutTheDatabase :{
                        href : "AboutTheDatabase",
                        value : "About The Database"
                    },
                    FormatAndTips : {
                        href : "formatAndTips",
                        value : "Format And Tips"
                    },
                    ContactUs : {
                        href : "http://www.landcareresearch.co.nz/about/contact/enquiry_form.asp?recipient_name=Maori%20Plant%20Use&recipient_imera=NgaTipuWhakaoranga",
                        value : "Contact Us"
                    }
                },
                user : {
                    userName : '',
                }
            }
        },
        methods :{
            goOut : function (url) {
                if (url.toString().indexOf('http') === -1 ) {
                    this.$parent.$refs.content.$refs.detail.currentComponent = url;
                    this.$parent.$refs.content.resize();
                } else if (url !== '' && url !== undefined) {
                    window.open(url) ;
                }
            },
            login : function () {
                this.$parent.$refs.loginFrame.loginFrame = 'login';
            }
        },
        mounted() {
        }
    }

</script>

<style scoped>
#default {
    background-color: rgba(166,171,167,1);
}

#red,#default {
    height: 140px;
    width: 100%;
    position: absolute;
}

#upperBar {
    height: 110px;
    width: inherit;
    background-color:  rgba(166,171,167,1);
    position: absolute;
    overflow: hidden;
}

#logo {
    float: left;
    height: 110px;
    width: 350px;
    background-color: inherit;
}
#logoDescription {
    height: 110px;
    width: 410px;
    font-size: 45px;
    text-align: center;
    line-height: 108px;
    position: relative;
    float: left;
}
#logoDescription p {
    line-height: 35px;
    text-align: left;
    margin: 0;
}

#logoPic {
    float: top;
    width: inherit;
    height: 75px;
}

#floatPic {
    height: 110px;
    width: 150px;
    float: right;
    background-color: bisque;
    position: relative;
}


/****************************************************************************************/

#bottomBar {
    width: inherit;
    height: 30px;
    background-color:  rgba(231,230,230,1);
    margin-top: 110px;
    overflow: hidden;
    text-align: center;
    border-color: black;
    border-top: 1px ;
    border-bottom: 1px ;
    border-left: 0 ;
    border-right: 0 ;
    border-style: solid;
}
#bottomBar >>> a {
    height: inherit;
    font-size: larger;
    font-weight: bold;
    font-family: "Times New Roman",serif;
    list-style-type:none;
    width: auto;
    min-width: 80px;
    float: left;
    text-align: center;
    line-height: 30px;
    display: block;
}
#bottomBar >>> a span {
    margin-right: 10px;
    margin-left: 10px;
}
#bottomBar >>> a:link,a:hover,a:visited,a:active {
    color: black;
}
#bottomBar >>> a:hover {
    text-decoration: none;
}
/*---------------------------------------------*/
#floatBar {
    position: fixed; right: 5px ; top: 140px; z-index: 999
}
</style>