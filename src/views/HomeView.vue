<template>
  <div
    class="d-none justify-center align-center"
    width="100vw"
    height="100vh"
    color="primary"
  >
    <v-card class="pa-2" width="300px" height="500px" color="primary" img="https://i.pinimg.com/564x/96/c3/72/96c372bbc2a4326cd4e88f34a6c48303.jpg">
      <v-text-field
        outlined
        label="Prepend inner"
        prepend-inner-icon="mdi-map-marker"
        v-model= "cityTemp"
        @keyup.enter = "changeCity"
      ></v-text-field>
    </v-card>
    <p>Weather at {{city}} is:</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      wind: "",
      cityTemp: ""
    }
  },
  methods: {
    changeCity() {
      this.city = this.cityTemp;
    }
  },
};
</script>

<style></style>
