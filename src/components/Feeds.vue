<template>
  <div class="hello">
    <!--Card-->
    <div v-for="post in postsFeed">
      <div class="container container-card" v-if="(post.brand+' '+post.model+' '+post.description).toUpperCase().indexOf(Search.toUpperCase()) > -1">
        <div class="card">
          <div class="card-content">
            <div class="media">
              <div class="media-left">
                <figure class="image crop w3-circle">
                  <img :src="post.photoUser" alt="Image">
                </figure>
              </div>
              <div class="media-content">
                <p class="title is-4">{{ post.username }}</p>
              </div>
            </div>
            <div class="card-image">
              <router-link :to="'/comments/' + id">
                <figure class="image is-4by3">
                  <img :src="post.photoShose" alt="" @click="getID (post['.key'])" width="300px">
                </figure>
              </router-link>
            </div>
            <div class="content" align="left">
              <strong>{{post.brand}} {{post.model}}</strong>
              {{post.description}}
              <i style="float: right"> 3.5 <span class="icon is-small"><i class="fa fa-star"></i></span></i>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--Card-->
  </div>
</template>

<script>
export default {
  props: ['posts', 'Search'],
  name: 'hello',
  data () {
    return {
      id: ''
    }
  },
  methods: {
    getID (id) {
      this.id = id
    }
  },
  computed: {
    postsFeed () {
      return this.posts.reverse()
    }
  }
}
</script>
