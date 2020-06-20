<template>
    <div class="map-container">
        <MglMap :mapStyle="mapStyle" :center.sync="centerCoordinates" :accessToken="accessToken" @load="onMapLoad">
            <MglMarker :key="index" v-for="(data, index) in  getMarkerData()" :coordinates.sync="data.coordinates"
                       color="blue">
                <MglPopup>
                    <b-card class="mb-4 text-left no-border no-padding">
                        <b-card-title>{{data.projectDetail.Title}}</b-card-title>
                        <b-card-sub-title>{{data.projectDetail.Category}}</b-card-sub-title>
                        <b-card-sub-title>{{data.projectDetail.SubCategory}}</b-card-sub-title>
                        <b-card-text>
                            <ul>
                                <li><b>Stage</b>: {{data.projectDetail.Value}}</li>
                                <li><b>Type</b>: {{data.projectDetail.Type}}</li>
                                <li><b>Value</b>: {{data.projectDetail.Value}}</li>
                                <li><b>Ownership</b>: {{data.projectDetail.Ownership}}</li>
                                <li><b>Notes</b>: {{data.projectDetail.Notes}}</li>
                                <li><b>Suburb</b>: {{data.projectDetail.Suburb}}</li>
                                <li><b>State</b>: {{data.projectDetail.State}}</li>
                                <li><b>Council</b>: {{data.projectDetail.Council}}</li>
                                <li><b>Address</b>: {{data.projectDetail.Address}}</li>
                            </ul>

                        </b-card-text>
                    </b-card>
                </MglPopup>
            </MglMarker>
        </MglMap>
    </div>
</template>

<script>
    import {MglMap, MglMarker, MglPopup} from "vue-mapbox";

    export default {
        name: 'MapContainer',
        props: {
            mapGeoJson: [Object, Array]
        },
        components: {
            MglMap,
            MglMarker,
            MglPopup
        },
        methods: {
            async onMapLoad(event) {
                // Here we cathing 'load' map event
                const asyncActions = event.component.actions;
                await asyncActions.flyTo({
                    center: this.getCenter(),
                    zoom: 15,
                    speed: 1
                })
            },
            getMarkerData() {
                // generating markers for site map
                const markerData = [];
                const mapJson = this.mapGeoJson;
                if (typeof mapJson.features.length !== 'undefined') {
                    for (let i = 0; i < mapJson.features.length; i++) {
                        let coordinates = mapJson.features[i].geometry.coordinates;
                        let projectDetail = mapJson.features[i].properties.project;
                        markerData.push({
                            coordinates: coordinates,
                            projectDetail: projectDetail
                        });
                    }
                }
                return markerData;
            },
            getCenter() {
                const coordinates = this.getMarkerData();
                return coordinates[0].coordinates
            }
        },
        data() {
            return {
                accessToken: 'pk.eyJ1IjoiemFpbmhhc2htaTQ3IiwiYSI6ImNrYm15dXdidzB1czEyd3A5enc2MWMwankifQ.sxoTQAX2L7-EG-NAOb0HDA',
                mapStyle: 'mapbox://styles/mapbox/streets-v11',
                centerCoordinates: [0, 0],
                sourceId: 'sourceId',
                layerId: 'layerId',
                geoJson: {
                    type: 'geojson',
                    data: {
                        "id": "featureSource",
                        "type": this.mapGeoJson.type,
                        "features": this.mapGeoJson.features
                    },
                }
            };
        },
        created() {
        }
    }
</script>

<style scoped>
    .map-container {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 1;
    }

    .card {
        padding: 0;
        border: 0;
        margin: 13px 0 0 0 !important;
    }

    .card .card-body {
        padding: 0;
    }

    .card .card-body .card-title {
        font-size: 13px;
        font-weight: bold;
    }

    .card .card-body .card-subtitle {
        font-size: 12px;
        margin-bottom: 10px;
    }

    .card .card-body .card-text ul {
        padding: 0;
        margin: 0;
        list-style: none;
    }

    .card .card-body .card-text ul li {
        font-size: 11px;
    }

    .card .card-body .card-text ul li b {
        text-decoration: underline;
    }
</style>
