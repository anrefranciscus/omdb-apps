<template>
 <section id="header-home">
  <div class="brand">
    <strong>OMDB</strong>
    <router-link to="/bookmark" class="bookmark">
       <img src="https://dl.dropbox.com/s/s9fb1d9vughng58/bookmark-64.png?dl=2"/>
       <span>{{ bookmarkCount }}</span>
    </router-link>
 </div>
 <div class="intro">
    <h3>
       Find your 
       <br>
       favorite movies
    </h3>
 </div>

 <div class="search-wrapper">
  <div class="search-bar">
    <input
    type="text" placeholder="Type title here"/>
    <button type="button">
      <i class="fa fa-search"></i>
    </button>
  </div>
 </div>

 <div class="category-wrapper">
  <strong>Categories</strong>
 </div>
 </section>
</template>

<script>
import Card from '../components/Card.vue'
export default {
  name: 'Home',
  components: {
    Card
  }
}
</script>
<style lang="scss">
  @import './src/scss/view-styles/_home';
</style>