<template>
    <div>
        <div v-bind:id="homeContent"></div>
    </div>
</template>

<script>
    export default {
        name: "HomePage",
        data() {
            return {
                homeContent : 'homeContent',
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
    background-color: #291e50;
    display: inline-block;
    position: relative;
}
</style>