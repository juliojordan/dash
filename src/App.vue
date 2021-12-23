<template>
  <div id="app">
    <table class="charts-css bar show-labels">
      <tbody>
        <tr v-for="[key, value] in arr" :key="key">
          <th>{{ key }}</th>
          <td :style="`--size:${value / 10};`"></td>
        </tr>
      </tbody>
    </table>
    <button @click="toggle">{{ msg }}</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return { data: {}, timer: null };
  },
  created() {
    this.load();
  },
  destroyed() {
    clearInterval(this.timer);
  },
  computed: {
    arr() {
      return Object.entries(this.data);
    },
    msg() {
      return this.timer ? "Stop real time" : "Start real time";
    },
  },
  methods: {
    load() {
      axios
        .get("https://data.juliojordan.workers.dev/")
        .then(({ data }) => {
          this.data = data;
        })
        .catch((err) => {
          console.error(err);
        });
    },
    toggle() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      } else {
        this.timer = setInterval(this.load, 1000);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: "SF Mono", Monaco, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table.bar {
  height: 100px;
  max-width: 300px;
  margin: 0 auto;
}
</style>
