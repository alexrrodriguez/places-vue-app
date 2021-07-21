<template>
  <div class="home">
    <h1>New Place</h1>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Address:
      <input type="text" v-model="newPlaceParams.address" />
      <button v-on:click="createPlace()">Create Place</button>
    </div>
    <h1>All Places</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2>{{ place.name }}</h2>
      <p>Address: {{ place.address }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      places: [],
      newPlaceParams: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        console.log("places index", response);
        this.places = response.data;
      });
    },
    createPlace: function () {
      axios
        .post("http://localhost:3000/places", this.newPlaceParams)
        .then((response) => {
          console.log("place created", response);
          this.places.push(response.data);
          this.newPlaceParams = {};
        })
        .catch((error) => {
          console.log("place create error", error.response);
        });
    },
  },
};
</script>
