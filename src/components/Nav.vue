<template>
  <nav>
    <div class="title">
      <span>BOOLFIX</span>
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
      if (this.searchMovie == '')
        return this.$parent.APICall('boobs'), this.$parent.APICallTv('boobs');
      return this.$parent.APICall(this.searchMovie), this.$parent.APICallTv(this.searchMovie);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
nav {
  height: 40px;
  background-color: rgba(30, 30, 30, 1);
  display:flex;
  padding: 0 20px;
  justify-content: space-between;
  align-items: center;
  .title {
    color: red;
    height:100%;
    display: flex;
    align-items:center;
    padding-top: 10px; 
  }
}
</style>
