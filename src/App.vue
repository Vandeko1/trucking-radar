<template>
    <div id="app">
        <div class="map">
          <span v-if="loading">Завантаження...</span>
            <l-map :zoom="zoom" :center="center">
                <l-tile-layer :url="osmUrl" :attribution="attribution"/>
                <l-routing-machine :waypoints="waypoints"/>
                <l-polyline :lat-lngs="polyline.latlngs" :color="polyline.color"/>
                <l-marker :lat-lng="[48.872391, 24.613980]">
                    <l-icon :icon-size="dynamicSize" :icon-anchor="dynamicAnchor" icon-url="static/images/baseball-marker.png" />
                </l-marker>
                <l-marker :lat-lng="[48.872391, 24.613980]">
                    <l-icon :icon-anchor="staticAnchor" class-name="someExtraClass">
                        <div class="headline">{{ customText }}</div>
                        <img src="./components/bus.png" />
                    </l-icon>
                </l-marker>  
            </l-map>
        </div>
        <div class="controls">
            <map-controls />
        </div>
    </div>
</template>

<script>
import { latLng, icon } from "leaflet";
import { LMap, LTileLayer, LPolyline, /*LGeoJson,*/ LMarker, LIcon} from 'vue2-leaflet'
import LRoutingMachine from './components/LRoutingMachine'
import MapControls from './components/MapControls'

const attribution = '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
const osmUrl = 'http://{s}.tile.osm.org/{z}/{x}/{y}.png'

import axios from "axios";/*axios*/  
    
    

export default {
    components: { LMap, 
                 LTileLayer, 
                 LRoutingMachine, 
                 MapControls, 
                 LPolyline,
                 LMarker,
                 LIcon 
    },
    data () {
        return {
            loading: false,
            zoom: 12,        
/*****************marker*icon*****************************/
//            icon: icon({
//                iconUrl: "./components/bus.png",
//                iconSize: [32, 37],
//                iconAnchor: [16, 37]
//            }),
            staticAnchor: [16, 37],
            customText: "Шкаврітко",
            iconSize: 64,
/****************time*dimension**************************/
            fullscreenControl: true,
            timeDimensionControl: true,
            timeDimensionControlOptions: {
                timeSliderDragUpdate: true,
                loopButton: true,
                autoPlay: true,
                playerOptions: {
                    transitionTime: 1000,
                    loop: true
                }
            },
            timeDimension: true,
/********** center: [36.72, -4.43]***************/
            center: { lat: 48.872391, lng: 24.613980 },
            osmUrl,
            attribution,
            marker: latLng(48.872391, 24.613980),/*marker*/
            waypoints: [
                { lat: 48.870391, lng: 24.611980 },
                { lat: 48.636334, lng: 24.571666 },
                { lat: 48.481873, lng: 24.587514 },
                { lat: 48.283773, lng: 24.565537 },
                { lat: 48.153289, lng: 24.818856 },
                { lat: 48.309955, lng: 25.073097 }
          ],
            polyline: {
            type: "polyline",
                latlngs: [
                    [48.870391, 24.611980],
                    [48.636334, 24.571666],
                    [48.481873, 24.587514],
                    [48.283773, 24.565537],
                    [48.153289, 24.818856],
                    [48.309955, 25.073097]
                ],
                color: "green"
            }
        }
    },
computed: {
    dynamicSize() {
        return [this.iconSize, this.iconSize * 1.15];
    },
    dynamicAnchor() {
        return [this.iconSize / 2, this.iconSize * 1.15];
    }
},    
created() {
    this.loading = true;
    axios
        .get("https://rawgit.com/gregoiredavid/france-geojson/master/regions/pays-de-la-loire/communes-pays-de-la-loire.geojson")
        .then(response => {
            this.geojson = response.data;
            this.loading = false;
        });
    }
}

</script>

<style>
@import "../node_modules/leaflet/dist/leaflet.css";

html, body, #app {
  height: 100%;
  margin: 0;
}
    #app {
        display: grid;
        grid-template-columns: 75% 25%;
        grid-template-rows: 100%;
    }
</style>