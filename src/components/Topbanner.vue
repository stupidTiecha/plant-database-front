<template>
    <div v-bind:id="topBar" >
        <div v-bind:id="upperBar" >
            <div v-bind:id="logo">
                <div v-bind:id="logoPic">
                    <a href="#"><img :src="logoPath" alt="这里是logo图片"></a>
                </div>
                <div v-bind:id="logoDescription">
                    <p style="display: inline-block ;
                    background-color: #161626;
                    height: 30px;
                    font-size: larger;
                    line-height: 30px;
                    margin-left: 8px ;
                    min-width: 176px;
                    color: #f2f2f4;
                    text-align: center">&nbsp;{{description}}&nbsp;</p>
                </div>
            </div>

            <div v-bind:id="floatPic" v-for="(path,index) in floatPics  " :key ='index'>
                <img :src="path" :alt="index + '号图片'"/>
            </div>
        </div>
        <div v-bind:id="bottomBar" >
            <a v-for="(select,key,slot) in selectBars" :key="key" href="#" v-on:click="goOut(select.href)">
                <span v-if="slot !== 0"> | </span>{{key}}
            </a>
            <a href ="#" v-on:click ="login" style="float: right">Log in</a>
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
                //这里是logo图片路径
                logoPath : require('../../public/logo-1.png'),
                //这里是logo下的介绍
                description : 'Māori Plant Use',
                //这里是头部浮动的图片地址，可以添加多个
                floatPics : [require('../../public/plant001.jpg'),require('../../public/plant002.jpg'),require('../../public/plant003.jpg')],
                //这里横条选择,添加时按照格式即可
                selectBars : {
                    DATABASE : {
                        //外部链接地址
                        href : 'http://www.landcareresearch.co.nz/databases/index.asp'
                    },
                    PLANTS : {
                        href : "http://www.landcareresearch.co.nz/databases/db_category.asp?DB_Coll_Category_ID=2"
                    },
                    HOME : {
                        href : "home"
                    }
                }
            }
        },
        methods :{
            goOut : function (url) {
                if (url === 'home') {
                    this.$parent.$refs.content.$refs.detail.currentComponent = 'homePage';
                } else if (url !== '' && url !== undefined) {
                    window.open(url) ;
                }
            },
            login : function () {
                this.$parent.$refs.loginFrame.loginFrame = 'login';
            }
        }
    }

</script>

<style scoped>
#default {
    background-color: #C0BDAA;
}

#red,#default {
    height: 140px;
    width: 100%;
    position: absolute;
}

#upperBar {
    height: 110px;
    width: inherit;
    background-color:  #161626;
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
    float: top;
    height: 35px;
    width: inherit;
    font-size: larger;
    text-align: left;
    position: relative;
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
    float: left;
    background-color: bisque;
    position: relative;
}


/****************************************************************************************/

#bottomBar {
    width: inherit;
    height: 30px;
    background-color: #c0c0c8;
    margin-top: 110px;
    overflow: hidden;
}
#bottomBar a {
    height: inherit;
    list-style-type:none;
    width: auto;
    min-width: 80px;
    float: left;
    text-align: center;
    line-height: 30px;
    display: block;
}
#bottomBar a span {
    float: left;
    margin-right: 10px;
    margin-left: 10px;
}
#bottomBar a:link,a:hover,a:visited,a:active {
    color: #161626;
}
#bottomBar a:hover {
    text-decoration: none;
}

</style>