<template>
  <div>
    <div class="floors">
      <div class="floor-color"></div>
      <div v-for="(floor, index) in floors" :key="index" class="floor-item">
        <div style="padding-left: 5%; padding-top: 5%">
          <div class="floor-card">
            <h1>{{ floor.name }}</h1>
            <h5>{{ floor.type }}</h5>
            <h5 id="dynamic-id">
              <span id="floor-id">DynamicId:</span>
              <span>{{ floor.dynamicId }}</span>
            </h5>
            <button @click="toggleRoomVisibility(index)"
                    :class="floor.roomVisibility ? 'hide-button' : 'show-button'">
              {{ floor.roomVisibility ? 'Hide Rooms' : 'Show Rooms' }}
            </button>
           
          </div>
        </div>
          <room-data :rooms="floor.children" v-if="floor.roomVisibility"/>
      </div>
    
    </div>
  </div>
 
</template>

<script>
import roomData from './roomData.vue';
export default {
  components: { roomData },
  props: {
    floors: {
      type: Array,

    }
  },
  
  methods: {
    toggleRoomVisibility(index) {
   
      this.$set(this.floors[index], 'roomVisibility', !this.floors[index].roomVisibility);
    }
  }
};
</script>
<style>
#floor-id{
    color: #ffc107;
}
.floors{
    
    
    background-color: #fff;;
    width: 200%;
    margin-top: 50px;
    
    
    border-radius: 3%;

    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    padding-bottom: 50px
 
}
.floor-color{
    width: 100%;
    height: 15px;
    background-color: #ffc107;

}
.floor-card{
    
    background-color: #f2f5f8;
    width: 60%;
    border-radius:15px ;
    padding-left: 5%;
    
}
.show-button {
    background-color: #4CAF50; 
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    margin-bottom: 10%;
  }
  
  .show-button:hover {
    background-color: #45a049; 
  }
  
  .hide-button {
    background-color: #FFA500; 
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    margin-bottom: 10%;
  }
  
  .hide-button:hover {
    background-color: #FF8C00; 
  }



</style>