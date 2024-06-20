<template>
  <div>
    <div class="availability-percentage">
      Available Rooms: {{ availablePercentage.toFixed(2) }}%
    </div>
    <div class="rooms" v-for="(room, index) in rooms" :key="index">
      <div :class="['room', getRoomClass(room.occupation)]">
        <span>piece n {{ index }} </span>
        <span>{{ room.name }}</span>
        <span class="occupation">{{ room.occupation }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    rooms: Array,
  },
  data() {
    return {
      availablePercentage: 0,
    };
  },
  created() {
    this.rooms.forEach(room => {
      this.fetchOccupation(room);
    });
  },
  methods: {
    //get rooms occupations from api
    async fetchOccupation(room) {
      try {
        const response = await axios.get(`https://api-developers.spinalcom.com/api/v1/room/${room.dynamicId}/control_endpoint_list`);
        
        if (response.data[0]) {
          room.occupation = response.data[0].endpoints[4].currentValue;
        } else {
          room.occupation = "undefined";
        }
        
       
        this.$forceUpdate();
        

        this.calculatePercentage();
      } catch (error) {
        console.error(`Error fetching occupation for room ${room.name}:`, error);
        room.occupation = "undefined"; 
      }
    },
    getRoomClass(occupation) {
      if (occupation === true) {
        return 'available';
      } else if (occupation === false) {
        return 'unavailable';
      } else {
        return 'undefined';
      }
    },
    //get availble rooms perceentage
    calculatePercentage() {
      const totalRooms = this.rooms.length;
      const availableRooms = this.rooms.filter(room => room.occupation === true).length;
      this.availablePercentage = (availableRooms / totalRooms) * 100;
    }
  },
};
</script>

<style>
.availability-percentage {
  margin-bottom: 10px;
  font-weight: bold;
}
.rooms {
  margin-top: 10px;
  margin-left: 10px;
  width: 80%;
}

.room {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);    
  border-radius: 3%;
  margin-bottom: 10px;
  height: 35px;
  padding-left: 10px;
  display: flex;
  align-items: center;
}

.available {
  background-color: #70ec95; 
}

.unavailable {
  background-color: #ff6b6b; 
}

.undefined {
  background-color: #f0e68c; 
}

.occupation {
  margin-left: auto;
  padding-right: 10px;
}
.availability-percentage{
  margin-left:120px;
  margin-top: 10px;
}
</style>
