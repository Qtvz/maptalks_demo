<template>
    <div>
        <Map id="map" class="container" ref="base"></Map>
    </div>
</template>

<script>
    import Map from '../../components/maptalks'
    export default {
        components: {Map}
    }
</script>

<style scoped>
.container {
    width: 100%;
    height: 90vh;
}
.container >>> .maptalks-attribution {
    display: none;
}
</style>