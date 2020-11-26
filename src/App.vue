<template>
    <div id="app">
        <l-map :zoom="zoom" :center="center">
            <l-tile-layer :url="osmUrl" :attribution="attribution" />
            <l-routing-machine :waypoints="waypoints" :key="key"></l-routing-machine>
        </l-map>
        <Select id="select" v-on:select="setSelected"/>
    </div>
</template>

<script>
    import { LMap, LTileLayer } from "vue2-leaflet";

    import LRoutingMachine from "./components/LRoutingMachine.vue";
    import Select from './components/Select'

    const attribution =
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors';
    const osmUrl = "http://{s}.tile.osm.org/{z}/{x}/{y}.png";
    export default {
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
                waypoints: [{
                        lat: 48.880007,
                        /*vxauto*/
                        lng: 24.630000
                    },
                    {
                        lat: 48.91,
                        /*мазепи для прикладу*/
                        lng: 24.69
                    }
                ],
                geo: {
                    '1': [{
                            time: '12-03-2020T12-51-36',
                            lat: 48.901230,
                            lng: 24.682841
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.899831,
                            lng: 24.693435
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.895190,
                            lng: 24.710685
                        }
                    ],
                    '2': [{
                            time: '12-03-2020T12-51-36',
                            lat: 48.894702,
                            lng: 24.714317
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.898516,
                            lng: 24.729848
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.906107,
                            lng: 24.735518
                        }
                    ],
                    '3': [{
                            time: '12-03-2020T12-51-36',
                            lat: 48.907155,
                            lng: 24.734986
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.912831,
                            lng: 24.738344
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.916075,
                            lng: 24.742664
                        }
                    ],
                    '4': [{
                            time: '12-03-2020T12-51-36',
                            lat: 48.923137,
                            lng: 24.742742
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.929930,
                            lng: 24.742309
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.931909,
                            lng: 24.739116
                        }
                    ],
                    '5': [{
                            time: '12-03-2020T12-51-36',
                            lat: 48.934216,
                            lng: 24.731103
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.932189,
                            lng: 24.728757
                        },
                        {
                            time: '12-03-2020T12-51-36',
                            lat: 48.931316,
                            lng: 24.726286
                        }
                    ],
                }
            };
        },
        methods: {
            setSelected: function(selected) {
                this.waypoints[1].lat = this.geo[selected.code][0].lat;
                this.waypoints[1].lng = this.geo[selected.code][0].lng;
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
        left: 0;
        z-index: 999;
        background-color: rgba(255,255,255,0.7);
        padding: 10px;
        height: 230px;
        width: 200px;
    }
    * {
        color: #555;
    }
</style>