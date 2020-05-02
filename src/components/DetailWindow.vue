<template>
    <div v-bind:id="detailWindow">
        <component v-bind:is="currentComponent" ref="childWindow"></component>
    </div>
</template>

<script>
    import HomePage from "./HomePage";
    import Search from "./Search";
    import AboutTheDatabase from "./AboutTheDatabase";
    import FormatAndTips from "./FormatAndTips";
    import ContactUs from "./ContactUs";

    export default {
        name: "DetailWindow",
        components: {HomePage,Search,AboutTheDatabase,FormatAndTips,ContactUs},
        data() {
            return {
                detailWindow : 'default',
                homePage : 'homePage',
                currentComponent : 'HomePage',
            }
        },
        methods : {

        }
    }
</script>

<style scoped>
#default{

}
</style>