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
    v-model="keyWord"
    v-on:keyup.enter="getData"
    type="text" placeholder="Type title here"/>
    <button @click="getData" type="button">
      <i class="fa fa-search"></i>
    </button>
  </div>
 </div>

 <div class="category-wrapper">
  <strong>Categories</strong>
  <div class="badge-wrapper">
    <div v-for="item in categorys" :key="item.id">
      <div
      @click="badgeClick(item.name)"
      :class="badgeActive === item.name ? 'active' : ''"
      class="badge">
        <img src="https://dl.dropbox.com/s/g35eijyruta8sv9/take-board-64.png?dl=2" alt="">
        <p>{{ item.name }}</p>
      </div>
    </div>
  </div>
 </div>

<EmptyState :isLoad="isLoad" :show-empty-false="showEmptyState" :is-empty-state="isEmptyState"></EmptyState>
 <!-- Card -->
 <div class="card-wrapper" v-if="response && !isLoad">
  <strong>Search result for '{{ keywordText }}'</strong>
  <div v-for="item in response" :key="item.imdbID">
      <Card :title="item.Title" :poster="item.Poster" :id-movie="item.imdbID" :year="item.Year" :type="item.Type"/>
  </div>
 </div>
 </section>
</template>

<script>
import EmptyState from '../components/EmptyState.vue'
import Card from '../components/Card.vue'
import API from '../api/index'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Card,
    EmptyState
  },
  data() {
    return {
      categorys: [
        {name: 'Movie', id: 1},
        {name: 'Episode', id: 2},
        {name: 'Series', id: 3},
      ],
      bookmarkCount: 0,
      badgeActive: 'Movie',
      keyWord: '',
      keywordText: '',
      response: '',
      responseStatus: 'true',
      isEmptyState: true,
      showEmptyState: true,
      isLoad: false,
      errorMsg: null,
      key: API.omdb.key,
      endPoint: API.omdb.endpoint
      
    }
  },
  watch: {
    
  },
  mounted(){
    if (localStorage.getItem('listMovie_omdb')){
      this.response = JSON.parse(localStorage.getItem('listMovie_omdb'))
      this.responseStatus = 'True',
      this.keywordText = localStorage.getItem('lastKeyWord_omdb')
    }
    if (localStorage.getItem('listBookmark_omdb')){
      let bookmark = JSON.parse(localStorage.getItem('listBookmark_omdb')).bookmark
      this.bookmarkCount = bookmark.length
    }
    
  },
  methods: {
    getData(){
      const validKeyWord = this.keyWord.split(' ').join('+')
      if (validKeyWord.split('').length > 0){
        this.showLoader(true)

        axios.get(`${this.endPoint}?apikey=${this.key}&s=${validKeyWord}&type=${this.badgeActive}`)
        .then( res => {
          console.log(res)
          this.response = res.data.Search
          this.responseStatus = res.data.Response
          this.keywordText = this.keyWord

          this.showLoader(false)

          if(this.responseStatus === 'True'){
            localStorage.setItem('listMovie_omdb', JSON.stringify(res.data.Search))
            localStorage.setItem('lastKeyWord_omdb', this.keyWord)
          }

        })
        .catch(err => this.errorMsg = err)
      }
    },
    showLoader(show){
      if (show) {
        this.showEmptyState = true
        this.isLoad = true
      }else {
        this.showEmptyState = false
        this.isLoad = false
      }
    },
    badgeClick(name){
      this.badgeActive = name
      this.getData()
    }
  }
}
</script>
<style lang="scss">
  @import './src/scss/view-styles/_home';
</style>