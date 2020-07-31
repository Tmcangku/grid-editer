<template>
  <div ref="map">
      <h1>我是标题</h1>
      <div>{{message}}</div>
  <div>{{propData}}</div>
  </div>
</template>
<script>
export default{
    name:'grid-editer',
    data(){
        return {
            message:'123456789'
        }
    },
    mounted(){
        
        console.log(8888);
        console.log(this.$refs.map);
    },
props:{
    propData:{
        type:String,
        default:'@室内地图'
    }
},
}
</script>
<style>

</style>