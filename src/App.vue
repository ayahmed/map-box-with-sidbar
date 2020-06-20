<template>
    <div id="app">
        <SideBar :mapJson="geoJsonData" />
        <MapContainer :mapGeoJson="filteredData" />
    </div>
</template>

<script>
    import SideBar from './components/SideBar.vue'
    import MapContainer from './components/MapContainer.vue'

    import axios from 'axios';

    import 'bootstrap/dist/css/bootstrap.css'
    import 'bootstrap-vue/dist/bootstrap-vue.css'

    export default {
        name: 'App',
        components: {
            SideBar,
            MapContainer
        },
        methods: {
            loadJson() {
                axios.get('https://bitbucket.org/idda/coding-challenges/raw/88be221f75a1b108c9e5f7222906b2735c147ac8/testBlob.json')
                    .then(response => {
                        this.geoJsonData  = response.data;
                        this.filteredData  = this.geoJsonData;
                    });
            }
        },
        data() {
            return {
                geoJsonData: [],
                filteredData: []
            }
        },
        mounted() {
            this.loadJson();
        },
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
