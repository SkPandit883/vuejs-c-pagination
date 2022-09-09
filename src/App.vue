<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <c-pagination
      :totalPages="10"
      :perPage="2"
      :currentPage="currentPage"
      activeColor="red"
      disableBg="green"
      height="50px"
      width="50px"
      fontSize="20px"
      color="purple"
      :maxVisibleButtons="4"
      @pagechanged="onPageChange"
    />
</template>

<script>
import CPagination from './components/CPagination.vue'

export default {
  name: 'App',
  components: {
    CPagination

  },
  data () {
    return {
      currentPage: 1,
    };
  },
  methods: {
    onPageChange(page) {
      console.log(page)
      this.currentPage = page;
    }
  }
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
