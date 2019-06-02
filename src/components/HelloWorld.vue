<template>
  <div class="hello">
    <button @click="getColor">Get A Color!</button>
    <p>
      <span>{{hex}}</span>
    </p>
    <div id="color-block" :style="{ backgroundColor:`${hex}` }"></div>
    <section id="historical-colors">
      <button @click="sortNewestFirst">^ Newest</button>
      <button @click="sortOldestFirst">^ Oldest</button>
      <table>
        <tr v-for="color in historicalColors" :key="color.hex" :style="{backgroundColor:color.hex}">
          <td>{{color.hex}}</td>
          <td>{{new Date(color.timestamp).toLocaleString()}}</td>
        </tr>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      response: '',
      hex: "",
      historicalColors: []
    };
  },
  methods: {
    getColor() {
      fetch("http://localhost:5001/generate-color")
        .then(response => {
          fetch("http://localhost:5001/get-all-colors")
          .then(response => response.json())
          .then(data => this.historicalColors = data.colors)
        return response.json();
      }).then(data => {
        this.hex = `${data.color.hex}`;
      });
    },
    sortNewestFirst() {
      this.historicalColors.sort((a, b) => a.timestamp < b.timestamp ? 1 : -1);
    },
    sortOldestFirst() {
      this.historicalColors.sort((a, b) => a.timestamp > b.timestamp ? 1 : -1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

#color-block {
  height: 200px;
  width: 200px;
}
</style>
