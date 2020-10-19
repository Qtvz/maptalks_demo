<template>
    <div>
        <div id="map" class="container"></div>
    </div>
</template>

<script>
    import 'maptalks/dist/maptalks.css'
    import * as maptalks from 'maptalks'
    import {WMTSTileLayer} from 'maptalks.wmts'
    export default {
        data() {
            return {
                map: null,
                zoom: 5,
                center: [104.493475, 35.058981],
                mapUrl: 'http://t{s}.tianditu.gov.cn/vec_c/wmts?tk=', // 矢量底图
                signUrl: 'http://t{s}.tianditu.gov.cn/cva_c/wmts?tk=', // 矢量注记
                imgUrl: 'http://t{s}.tianditu.gov.cn/img_c/wmts?tk=', // 影像底图
                ciaUrl: 'http://t{s}.tianditu.gov.cn/cia_c/wmts?tk=', // 影像注记
                iboUrl: 'http://t{s}.tianditu.gov.cn/ibo_c/wmts?tk=',//全球境界
                webKey: '5eaf957b45262af2e319cc061b338432'
            }
        },
        mounted() {
            this.initMap()
        },
        methods: {
            initMap() {
                const mapOptions = {
                    center: this.center,
                    zoom: this.zoom,
                    spatialReference: {
                        projection: 'EPSG:4326'
                    },
                    scaleControl: {
                        position: 'bottom-left',
                        maxWidth: 100,
                        metric: true,
                        imperial: false
                    },
                    zoomControl: {
                        position: 'top-right',
                        slider: false,
                        zoomLevel: false
                    },
                    baseLayer: new WMTSTileLayer('base',{
                        tileSystem: [1, -1, -180, 90],
                        layer: 'vec',
                        tilematrixset: 'c',
                        format: 'tiles',
                        urlTemplate: this.mapUrl + this.webKey,
                        subdomains: ['0', '1', '2', '3', '4', '5', '6', '7']
                    }),
                    layers:[
                        new WMTSTileLayer('sign',{
                            tileSystem: [1, -1, -180, 90],
                            layer: 'cva',
                            tilematrixset: 'c',
                            format: 'tiles',
                            urlTemplate: this.signUrl + this.webKey,
                            subdomains: ['0', '1', '2', '3', '4', '5', '6', '7']
                        }),
                        new WMTSTileLayer('ibo',{
                            tileSystem: [1, -1, -180, 90],
                            layer: 'ibo',
                            tilematrixset: 'c',
                            format: 'tiles',
                            urlTemplate: this.iboUrl + this.webKey,
                            subdomains: ['0', '1', '2', '3', '4', '5', '6', '7']
                        })
                    ]
                }
                this.map = new maptalks.Map('map', mapOptions)
            }
        }
    }
</script>

<style scoped>
    .container{
        width:100%;
        height:90vh
    }
</style>