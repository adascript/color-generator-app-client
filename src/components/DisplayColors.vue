<template>
<div>
  <h1>Generate a color!</h1>
  <section class="generate-color">
    <div id="color-block" :style="{ backgroundColor:`${hex}` }"></div>
    <h2>{{hex}}</h2>
    <button @click="getColor">Get A Color!</button>
    </section>
    <section id="historical-colors">
      <h2>Colors generated since the beginning of time:</h2>
      <button @click="sortNewestFirst" :disabled="sortOrder == 'new'" >Newest First</button>
      <button @click="sortOldestFirst" :disabled="sortOrder != 'new'">Oldest First</button>
      <table>
        <thead>
          <tr>
            <th>Hex Code</th>
            <th>Time of Creation</th>
            <th>Preview</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="color in historicalColors" :key="color.hex">
            <td>{{color.hex}}</td>
            <td>{{new Date(color.timestamp).toLocaleString()}}</td>
            <td :style="{backgroundColor:color.hex}" class="color-preview"></td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  name: 'DisplayColors',
  data() {
    return {
      response: '',
      hex: "#000",
      historicalColors: [],
      sortOrder: "new"
    };
  },
  methods: {
    getColor() {
      fetch("https://color-app-generator-backend.herokuapp.com/generate-color")
        .then(response => {
          this.getHistoricalColors();
        return response.json();
      }).then(data => {
        this.hex = `${data.color.hex}`;
      });
    },
    sortNewestFirst() {
      this.sortOrder = "new";
      this.historicalColors.sort((a, b) => a.timestamp < b.timestamp ? 1 : -1);
    },
    sortOldestFirst() {
      this.sortOrder = "old";
      this.historicalColors.sort((a, b) => a.timestamp > b.timestamp ? 1 : -1);
    },
    getHistoricalColors() {
      return fetch("https://color-app-generator-backend.herokuapp.com/get-all-colors")
          .then(response => response.json())
          .then(data => {
            let sortedColors = [...data.colors];
            if (this.sortOrder == "new") {
              sortedColors.sort((a, b) => a.timestamp < b.timestamp ? 1 : -1);
            }
            this.historicalColors = sortedColors;
          }) 
    }
  },
  beforeMount() {
    this.getHistoricalColors().then(() => {
        let mostRecentColor = this.historicalColors[0];
        let mostRecentHex = mostRecentColor.hex;
        this.hex = mostRecentHex;
    });
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

table, th, td {
  border-left: none;
  border-right: none;
}

table {
  width: 60vw;
}

tr {
  height: 30px;
}

tbody>tr:hover {
  background-color: #999;
}

#color-block {
  height: 200px;
  margin: 60px auto;
  margin-left: auto;
  margin-right: auto;
}

.color-preview {
  width: 35%;
}

</style>
