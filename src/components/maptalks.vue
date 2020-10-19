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
                webKey: '5eaf957b45262af2e319cc061b338432',
                tileSub:['0', '1', '2', '3', '4', '5', '6', '7']
            }
        },
        mounted() {
            this.initMap()
        },
        methods: {
            initMap() {
                const baseOptions = {
                    layer: 'vec',
                    urlTemplate: this.mapUrl + this.webKey
                }
                const signOptions = {
                    layer: 'cva',
                    urlTemplate:this.signUrl + this.webKey
                }
                const iboOptions = {
                    layer: 'ibo',
                    urlTemplate: this.iboUrl + this.webKey,
                }
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
                    baseLayer: this.addWMTSLayer(baseOptions),
                    layers:[
                        this.addWMTSLayer(signOptions),
                        this.addWMTSLayer(iboOptions)
                    ]
                }
                this.map = new maptalks.Map('map', mapOptions)
            },
            addWMTSLayer(options) {
                return new WMTSTileLayer(options.layer,{
                    tileSystem: [1, -1, -180, 90],
                    layer: options.layer,
                    tilematrixset: 'c',
                    format: 'tiles',
                    urlTemplate: options.urlTemplate,
                    subdomains: this.tileSub
                })
            }
        }
    }
</script>

<style scoped>
    .container{
        width:100%;
        height:100%;
    }
</style>