<template>
    <div>
        <div v-bind:id="homeContent">
            <img src="../../public/home-page-pic.jpg"
                 alt="homePage-pic"
                 style="position: absolute; right: 0;left: 0">
            <div v-bind:id="note">
                <p style="line-height: 40px; font-size: 26px;letter-spacing:3px; text-align: left">
                    Māori Plant Use
                </p>
                <p style="width: 260px ; display: inline-block; text-align: left ;margin-top: 170px;margin-left: 10px ;letter-spacing:-1px; font-weight: 400; line-height: 18px" >
                    Easy access to detailed information on Māori traditional uses of New Zealand native plants
                </p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "HomePage",
        data() {
            return {
                homeContent : 'homeContent',
                note : 'note',
            }
        }
    }
</script>

<style scoped>
#homeContent{
    top : 30px;
    left : 30px;
    height: 400px;
    width: 650px;
    background-color: #feffdb;
    display: inline-block;
    position: relative;
}
#note {
    width: 320px;
    height: 350px;
    top: 50px;
    background-color: rgba(192,189,170,0.0);
    position: absolute;
    left: 330px;
}
</style>