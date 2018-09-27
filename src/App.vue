<template>
    <div id="app">

        <vue-icon-picker v-model="icon"/>
    </div>
</template>

<script>
    import VueIconPicker from './vue-icon-picker'

    export default {
        name: 'app',
        data: function () {
            return {
                icon: {name: 'newspaper', type: 'fontawesome',color:"#AD2727"}
            }
        },
        components: {
            VueIconPicker
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;

    }
</style>
