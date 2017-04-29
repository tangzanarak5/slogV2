<template>
  <div id="app">
    <div v-if="ready">
      <div v-if="authorized" >
        <img :src="profile.photoURL" alt="">
        <h1>{{ profile.displayName }}</h1>
        <button type="button" @click="logout" class="button is-danger">logout</button>
        <router-link to="/addpost">addpost</router-link>
      </div>
      <div v-else>
        <button type="button" @click="login" class="button is-primary">login</button>
      </div>
    </div>
    <div v-else>
      <button type="button" name="button" class="button is-primary is-loading">login</button>
    </div>
    <router-view :posts="posts" :addPost="addPost" :addComment="addComment" :profile="profile" :login="login"></router-view>
  </div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyBRctA3Eq4nm23rB5Ho48TmSwX-q1UGARQ',
  authDomain: 'slog-1d912.firebaseapp.com',
  databaseURL: 'https://slog-1d912.firebaseio.com',
  projectId: 'slog-1d912',
  storageBucket: 'slog-1d912.appspot.com',
  messagingSenderId: '203904123343'
}
var firebaseApp = firebase.initializeApp(config)
var storage = firebase.storage().ref('photoPost')
var db = firebaseApp.database()
var provider = new firebase.auth.FacebookAuthProvider()
export default {
  name: 'app',
  data () {
    return {
      profile: {
        name: 'fluke'
      },
      ready: false,
      authorized: false,
      posts: [],
      comments: [],
      photoShose: ''
    }
  },
  mounted () {
    var vm = this
    vm.$bindAsArray('posts', db.ref('posts'))
    firebase.auth().onAuthStateChanged(function (user) {
      if (user) {
        vm.authorized = true
        vm.profile = user
      }
      vm.ready = true
    })
  },
  methods: {
    addPost (newPost, file) {
      var vm = this
      storage.child(file.name + Date.now() + Math.floor((Math.random() * 1000) + 1)).put(file).then(function (snapshot) {
        vm.photoShose = snapshot.downloadURL
        vm.$firebaseRefs.posts.push({
          username: newPost.username,
          photoUser: newPost.photoUser,
          model: newPost.model,
          description: newPost.description,
          comments: newPost.comments,
          photoShose: vm.photoShose,
          brand: newPost.brand
        })
      })
    },
    addComment (id, newComment) {
      var vm = this
      vm.$bindAsArray('comments', db.ref('posts/' + id + '/comments'))
      vm.$firebaseRefs.comments.push({
        username: newComment.username,
        comment: newComment.comment
      })
    },
    login () {
      firebase.auth().signInWithRedirect(provider)
    },
    logout () {
      let vm = this
      firebase.auth().signOut().then(function () {
        vm.authorized = false
      }, function (error) {
        console.error(error)
      })
    }
  }
}
</script>
