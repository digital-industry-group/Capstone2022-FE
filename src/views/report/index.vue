<template>
  <div class="dashboard-container">
    <div class="text-center">

      <h2>All Report</h2><button class="btn" @click="loadReports">Refresh</button>
    </div>
    <hr>
    <p v-if="isLoading">Loading ...</p>
    <p v-else-if="!isLoading && error">
      {{ error }}
    </p>
    <p v-else-if="!isLoading && (!results || results.length === 0)">No results.</p>
    <div v-else-if="!isLoading && results && results.length > 0">
      <ol>
        <li v-for="result in results" :key="result.id">
          {{ result }}
        </li>
      </ol>

    </div>
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'

export default {
  name: 'Report',
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    }
  },
  // computed: {
  //     ...mapGetters([
  //         'reports'
  //     ])
  // },
  created() {
    this.loadReports()
  },
  methods: {
    loadReports() {
      this.isLoading = true
      this.error = null
      fetch('http://127.0.0.1:5000/reports').then((response) => {
        if (response.ok) {
          return response.json()
        }
      }).then((data) => {
        // console.log(data);
        this.isLoading = false
        const results = []
        for (const id in data['reports']) {
          results.push(data['reports'][id])
        }
        this.results = results
        console.log(results[0].list_stats.click_rate)
      })
        .catch((error) => {
          console.log(error)
          this.isLoading = false
          this.error = 'Failed to fetch data. Please try again later'
        })
    }
  }
}
</script>

<style scoped>

.text-center{
    text-align: center;
}
li{
    border: 1px solid #000;
    margin-bottom: 5px;
    padding: 5px;
}

.btn {
  background: #40C9C6;
  background-image: -webkit-linear-gradient(top, #40C9C6, #40C9C6);
  background-image: -moz-linear-gradient(top, #40C9C6, #40C9C6);
  background-image: -ms-linear-gradient(top, #40C9C6, #40C9C6);
  background-image: -o-linear-gradient(top, #40C9C6, #40C9C6);
  background-image: linear-gradient(to bottom, #40C9C6, #40C9C6);
  -webkit-border-radius: 28;
  -moz-border-radius: 28;
  border-radius: 28px;
  font-family: Arial;
  color: #ffffff;
  font-size: 20px;
  padding: 10px 20px 10px 20px;
  text-decoration: none;
  border: none;
}

.btn:hover {
  background: #398f8c;
  background-image: -webkit-linear-gradient(top, #398f8c, #398f8c);
  background-image: -moz-linear-gradient(top, #398f8c, #398f8c);
  background-image: -ms-linear-gradient(top, #398f8c, #398f8c);
  background-image: -o-linear-gradient(top, #398f8c, #398f8c);
  background-image: linear-gradient(to bottom, #398f8c, #398f8c);
  text-decoration: none;
}

</style>
