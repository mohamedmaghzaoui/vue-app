<template>
    <div>
      <div v-if="data.length">
        <h1>List of Buildings</h1>
        <div v-for="(building, index) in data" :key="index">
          <div class="building">
            <div id="card-color"></div>
            <div id="card-info">
              <h1 id="name">{{ building.name }}</h1>
              <h3 id="type">{{ building.type }}</h3>
              <p id="dynamic-id">
                <span id="building-id">DynamicId:</span>
                <span>{{ building.dynamicId }}</span>
              </p>
              <button @click="toggleVisibility(building)" v-if="building.isVisible" class="red-button">hide floors</button>
              <button @click="toggleVisibility(building)" v-else class="blue-button">show floors</button>
            </div>
            <floor-data :floors="building.children" v-if="building.isVisible" />
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import floorData from './floorData.vue';
  
  export default {
    components: { floorData },
    data() {
      return {
        data: []
      };
    },
    created() {
      this.fetchData();
    },
    methods: {
      toggleVisibility(building) {
        building.isVisible = !building.isVisible;
      },
      async fetchData() {
        let url = "https://api-developers.spinalcom.com/api/v1/geographicContext/space";
        try {
          const response = await axios.get(url);
          this.data = response.data.children.map(building => ({
            ...building,
            isVisible: false
          }));
        } catch (err) {
          console.log(err);
        }
      }
    }
  };
  </script>
  
  <style>
  .building {
    background-color: #fff;
    width: 20%;
    height: 230px;
    border-radius: 3%;
    margin-left: 100px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  #card-color {
    width: 100%;
    height: 15px;
    background-color: #ffc107;
  }
  #card-info {
    padding-left: 20px;
  }
  #building-id {
    color: red;
    font-weight: bolder;
  }
  .blue-button {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s ease;
  }
  .blue-button:hover {
    background-color: #0056b3;
  }
  .red-button {
    background-color: #dc3545;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s ease;
  }
  .red-button:hover {
    background-color: #b02a37;
  }
  </style>
  