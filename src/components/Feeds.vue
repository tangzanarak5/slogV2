<template>
  <div class="hello">
    <div v-for="post in postsFeed">
      <div class="">
        <div class="">
          <img :src="post.photoUser" alt="">{{post.username}}
        </div>
        <div class="">
          <div class="">
            <h1>{{post.brand}} {{post.model}}</h1>
          </div>
          <router-link :to="'/comments/' + id"><img :src="post.photoShose" alt="" @click="getID (post['.key'])" width="300px"></router-link>
          <br>
          {{post.description}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['posts'],
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
