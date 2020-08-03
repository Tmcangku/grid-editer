<template>
  <div ref="map" class="map">
  </div>
</template>
<script>
const L = require('leaflet');
require('../assets/leaflet.css');
const imageUrl = require('../assets/images/logo.png')

export default{
    name:'grid-editer',
    data(){
        return {
            message:'123456789'
        }
    },
    mounted(){
        const map = this.initMap(this.$refs.map);
        this.loadImg(map,imageUrl);

    },
props:{
    propData:{
        type:String,
        default:'@室内地图'
    },
    girdShow:{
        type:Boolean,
        default:true 
    }
},
methods:{
        initMap(mapDom){
        const map = new L.Map(mapDom, {
            center: [39.92, 116.46],
            minZoom: 1,
            maxZoom: 6,
            attributionControl: false,
            crs: L.CRS.Simple
        });
        L.control
            .attribution({
                position: "bottomright",
                prefix: this.propData
            })
            .addTo(map);
        return map;
    },
    loadImg(map,imageUrl){
        if (this.imageLayer) {
            this.imageLayer.remove();
        }
            const img = new Image();
            img.src = imageUrl;
            img.onload = () => {
                let w = img.width,
                    h = img.height,
                    southWest = map.unproject([0, h], map.getMaxZoom() - 1),
                    northEast = map.unproject([w, 0], map.getMaxZoom() - 1);
                    const mapWidth = northEast.lng -  southWest.lng;
                    const proportion = w/mapWidth;
                this.imageBounds = new L.LatLngBounds(southWest, northEast);
                this.imageLayer = L.imageOverlay(imageUrl, this.imageBounds, {
                    opacity: 0.5
                }); //opacity是透明度
                map.addLayer(this.imageLayer);
                map.fitBounds(this.imageLayer.getBounds());
              if(this.girdShow) this.lineCreatFun(this.imageBounds,map);
               
        }
    },
     lineCreatFun(imageBounds,map) {
         console.log(map);
        if(imageBounds && imageBounds._northEast){
        const posEas = map.project(imageBounds._northEast,map.getMaxZoom() - 1);//【w，0】
        const posWest = map.project(imageBounds._southWest,map.getMaxZoom() - 1);//【0,h】
        const line = [];
        for (let i = posWest.x; i < posEas.x; i+=10) {
            const latlngs = [
                map.unproject([i, posWest.y], map.getMaxZoom() - 1),
                map.unproject([i, posEas.y], map.getMaxZoom() - 1) ,
            ];

            line.push(
                L.polyline(latlngs, {
                    color: "#fff",
                    weight: 1,
                    opacity: 0.5
                })
            );
        }

        for (let i = posEas.y; i < posWest.y; i+=10) {
            const latlngs = [
                map.unproject([posWest.x, i], map.getMaxZoom() - 1),
                map.unproject([posEas.x, i], map.getMaxZoom() - 1) ,
            ];
            line.push(
                L.polyline(latlngs, {
                    color: "#fff",
                    weight: 1,
                    opacity: 0.5
                })
            );
        }
        this.GroupLayer = L.layerGroup(line);
        this.GroupLayer.addTo(map);
        }
    },

}
}
</script>
<style>
.map{
    width: 100%;
    height: 100%;
}
</style>