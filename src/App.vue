<template>
  <div id="app">
    <Building-data/>
    
    
    
    
  </div>
</template>

<script>
import BuildingData from './components/buildingData.vue'



export default {
  name: 'App',
  components: {
    BuildingData
    
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  
  color: #2c3e50;
  margin-top: 60px;

  
  

}
body {
  background-color: #f2f5f8;
}

</style>
