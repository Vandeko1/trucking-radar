<template>
    <div id="app">
        <l-map :zoom="zoom" :center="center">
            <l-tile-layer :url="osmUrl" :attribution="attribution" />
            <l-routing-machine :waypoints="waypoints" :key="key"></l-routing-machine>
        </l-map>
        <Select id="select" v-on:select="setSelected" :vehicles="vehicles"/>        
    </div>
</template>
<script>
    import { LMap, LTileLayer } from "vue2-leaflet";
    import LRoutingMachine from "./components/LRoutingMachine.vue";
    import Select from './components/Select'
    import axios from 'axios';    
    const attribution = '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors';
    const osmUrl = "http://{s}.tile.osm.org/{z}/{x}/{y}.png";
    export default {
        name: "VXradar",
        components: {
            LMap,
            LTileLayer,
            LRoutingMachine,
            Select
        },
        data() {
            return {
                key: 1,
                value: null,
                zoom: 6,
                center: {
                    lat: 48.87,
                    lng: 24.62
                },
                osmUrl,
                attribution,
                waypoints: [
                    { lat: 48.91,
                      lng: 24.69 },
                    { lat: 48.880007,
                      lng: 24.630000}
                ],
                data: new Array(),
                geo: new Array(),
                vehicles: new Array()
            };
        },
        mounted() {
            axios
              .get('http://localhost:3000/')
              .then(response => { 
                let data = response.data;
                let length = data.length;
                for (let i = 0; i < length; i++) {
                    this.vehicles.push({ 
                        label: data[i].name, 
                        code: data[i].code 
                    });
                    this.geo.push({ 
                        lat: data[i].lat, 
                        lng: data[i].lng,
                        code: data[i].code  
                    });
                }
              })
              .catch(error => {console.log(error)})            
        },
        methods: {
            setSelected: function(selected) {
                const route = this.geo.find(obj => obj.code === selected.code);
                
                this.waypoints[0].lat = route.lat;
                this.waypoints[0].lng = route.lng;
                
//                console.log(`lat:${route.lat} lng:${route.lng}`);
                this.key++;
            }
        }
    };
</script>

<style>
    @import "../node_modules/leaflet/dist/leaflet.css";
    @import "../node_modules/vue-select/dist/vue-select.css";

    html,
    body,
    #app {
        height: 100%;
        margin: 0;
    }
    #lmap {
        height: 100%;
        width: 100%;
    }
    #select {
        position: absolute;
        bottom: 0;
        right: 0;
        z-index: 999;
        background-color: rgba(255,255,255,0.7);
        padding: 10px;
        height: 500px;
        width: 300px;
    }
    * {
        color: #555;
    }
</style>