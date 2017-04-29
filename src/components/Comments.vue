<template lang="html">
<div class="">
  <div v-for="post in posts">
    <div v-show="post['.key'] == id">
      <img :src="post.photoUser" alt="">{{post.username}}
      <br>
      <div class="">
        <div class="">
          <h1>{{post.brand}} {{post.model}}</h1>
        </div>
        <img :src="post.photoShose" alt="" width="300px">
        <br>
        {{post.description}}
      </div>
      <div v-for="comment in post.comments">
        {{comment.comment}}
        {{comment.username}}
      </div>
      <div v-if="this.profile != null">
        <input type="text" v-model="textComment" @keyup.enter="comment">
      </div>
      <div v-else>
        <button type="button" @click="loginForComment">login for comment</button>
      </div>
    </div>
  </div>
</div>
</template>
<script>
import firebase from 'firebase'
export default {
  props: ['id', 'posts', 'addComment', 'profile', 'login'],
  data () {
    return {
      textComment: ''
    }
  },
  methods: {
    comment () {
      var newComment = {
        photoUser: this.profile.photoURL,
        username: this.profile.displayName,
        comment: this.textComment
      }
      this.addComment(this.id, newComment)
    },
    loginForComment () {
      this.login()
    }
  },
  mounted () {
    console.log('id' + this.id)
    var Comments = firebase.database().ref('post/' + this.id + '/comments')
    Comments.on('child_added', function (snapshot) {
      var itemSlog = snapshot.val()
      itemSlog.id = snapshot.key
      // console.log(snapshot)
      // .push(itemSlog)
    })
  }
}
</script>

<style lang="css">
</style>
