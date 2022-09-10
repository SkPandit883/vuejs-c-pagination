<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->

  <table class="table table-striped">
    <thead>
      <tr>
        <th scope="col">SN</th>
        <th scope="col">ID</th>
        <th scope="col">Company Name</th>
        <th scope="col">Contact Name</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(tour,key) in tours.data" :key="key">
        <th scope="row">{{key+1}}</th>
        <td>{{tour.id}}</td>
        <td>{{tour.company_name}}</td>
        <td>{{tour.contact_name}}</td>
      </tr>

    </tbody>
  </table>

  <c-pagination :totalPages="total" :perPage="2" :currentPage="currentPage" activeColor="green"
    height="50px" width="50px" fontSize="20px" color="black" @pagechanged="onPageChange" />
</template>

<script>
import CPagination from './components/CPagination.vue'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    CPagination

  },
  data() {
    return {
      currentPage: 1,
      total:10,
      tours: {},
    };
  },
  methods: {
    onPageChange(page) {
      this.fetchTours({ "per_page": 2, 'page': page })
      this.currentPage = page;
    },
    fetchTours(params) {
      axios.get(`https://www.actionfurnituresansar.com/api/tour/fetch-all?per_page=${params.per_page}&page=${params.page}`)
        .then(response => {
          this.tours = response.data.tour_companies
          this.currentPage = response.data.tour_companies.meta.current_page
          this.total = response.data.tour_companies.meta.last_page
          console.log('total', this.total)
          console.log('currentPage', this.currentPage)
        })
        .catch(error => console.log(error))
    }
  },
  mounted() {
    this.fetchTours({ "per_page": 2, 'page': 1 })
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
