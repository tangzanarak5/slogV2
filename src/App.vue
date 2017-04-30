<template>
  <div id="app">
    <!--Navbar-->
    <nav class="nav has-shadow ">
      <div class="container container-nav">
        <div class="nav-left">
          <router-link to="/" class="nav-item"><img src="../images/logo.png"></router-link>
        </div>
        <div class="nav-center nav-item">
          <p class="control has-icons-left has-icons-right">
            <input class="input" type="text" placeholder="Search" v-model="Search">
            <span class="icon is-small is-left">
              <i class="fa fa-search"></i>
            </span>
          </p>
        </div>
        <div class="nav-right nav-menu">
          <a class="nav-item is-tab" v-if="authorized">
            <figure class="image is-16x16" style="margin-right: 8px;">
              <img :src="profile.photoURL">
            </figure>
            {{ profile.displayName }}
          </a>
          <a class="nav-item is-tab" v-if="authorized" @click="logout">
            <span class="fa fa-sign-out"> Log out</span>
          </a>
          <a class="nav-item is-tab" v-else @click="login">
            <span class="fa fa-sign-in" v-if="ready"> Log in</span>
            <span class="fa fa-spinner w3-spin" v-else></span>
          </a>
        </div>      
      </div>
    </nav>
    <!--Navbar-->
    <!--Add Post-->
    <span class="icon is-large icon-add" v-if="authorized">
      <router-link to="/addpost"><i class="fa fa-plus-circle" aria-hidden="true"></i></router-link>
    </span>
    <!--Add Post-->
    <router-view :posts="posts" :addPost="addPost" :addComment="addComment" :profile="profile" :login="login" :authorized="authorized" :Search="Search"></router-view>
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
      Search: '',
      profile: {},
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
        photoUser: newComment.photoUser,
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
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.container {
  margin: auto;
}
.container-nav {
  width: 80%;
}
.container-card {
  width: 40%;
}
.container-card .card {
  margin-top: 16px;
}
.modal-content {
  width: 80%;
  height: 560px;
  background-color: #000;
}
.card-image, #image-post {
  margin: auto;
}
.section-header {
  position: relative;
  height: 88px;
  padding: 0;
  border-bottom: 1px;
  border-bottom: 1px solid #eee;
}
.section-content {
  height: 327px;
  padding: 0.5rem;
}
.section-content .box {
  margin-bottom: 0.75rem;
}
.section-footer {
  height: 145px;
  padding: 0;
}
.box {
  padding: 0.75em;
}
.comment-box {
  height : 88px;
  width : 95%;
  resize: none;
  overflow: none;
  padding: 0;
}
.send-btn {
  position: relative;
  margin-right: 10px;
  right: 0;
}
.img-circle {
  border-radius: 50%;
}
.icon-add {
  position: fixed;
  right: 32px;
  bottom: 32px;
}
.nav, .card, .modal .name-profile, .modal .name-profile-header {
  font-family: 'Sriracha', cursive;
}
.modal .name-profile-header {
  font-size: 1.25em;
}
.crop {
    width: 64px;
    height: 64px;
    overflow: hidden;
}

.crop img {
    width: auto;
    height: auto;
}
</style>

