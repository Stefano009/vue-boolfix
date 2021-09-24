<template>
  <nav>
    <div class="title">
      <h3>BOOLFIX</h3>
    </div>
    <div class="search">
      <input @keyup.enter="userQuery" v-model="searchMovie" type="text">
      <button @click="userQuery" >Search</button>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Nav',
  data() {
    return{
      searchMovie: '',
    }
  },
  methods: {
    userQuery() {
      return this.$parent.APICall(this.searchMovie);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
