<template>
  <div id="app">
    <NavBar />
    <div v-for="(data, index) in data.rows" :key="index">
      <SetupData :data="data" />
    </div>
  </div>
</template>

<script>
import jsondata from "./config.json";
import SetupData from "./components/SetupData.vue";
import NavBar from "./components/NavBar.vue";

export default {
  name: "App",

  data() {
    return {
      data: jsondata,
    };
  },

  components: {
    SetupData,
    NavBar,
  },
};
</script>
