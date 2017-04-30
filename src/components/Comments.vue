<template lang="html">
<div class="">
    <!--Modal-->
    <div v-for="post in posts">
      <div class="modal is-active" v-if="post['.key'] === id">
        <router-link :to="'/'">
          <div class="modal-background"></div>
        </router-link>
        <div class="modal-content">
          <div class="columns is-gapless is-multiline">

            <div class="column is-two-thirds" id="image-post">
              <img :src="post.photoShose" alt="Image">
            </div>

            <div class="column is-one-third">
              <section class="section section-header">
                <div class="media" style="padding: 16px;">
                  <div class="media-left">
                    <figure class="image crop w3-circle">
                      <img :src="post.photoUser" class="">
                    </figure>
                  </div>
                  <div class="media-content">
                    <h1 class="name-profile-header">{{post.username}}</h1>               
                    <p> 4 <span class="icon is-small"><i class="fa fa-star" aria-hidden="true"></i></span></p>
                  </div>
                </div>
              </section>
              <section class="modal-card-body section-content">

                <div class="box">
                  <article class="media">
                    <div class="media-content">
                      <div class="content">
                        <p>
                          <span class="">{{post.brand}} {{post.model}}</span>
                          <br>
                          {{post.description}}
                        </p>
                      </div>
                    </div>
                  </article>
                </div>
                
                <div v-for="(comment, key) in post.comments">
                  <div class="box" v-show="key != '0'">
                    <article class="media">
                      <div class="media-left">
                        <figure class="image is-48x48">
                          <img :src="comment.photoUser" alt="Image">
                        </figure>
                      </div>
                      <div class="media-content columns">
                        <div class="content column is-8">
                          <p>
                            <span class="name-profile">{{comment.username}}</span>
                            <nav class="level is-mobile">
                              <div class="level-left">
                                <star-rating :increment="0.5" :read-only="true" :star-size="20" :rating="comment.rating"></star-rating>
                              </div>
                            </nav>
                            <br>
                            <p><div style="word-wrap: break-word; width:100%;">{{comment.comment}}</div></p>
                          </p>
                        </div>
                      </div>
                    </article>
                  </div>
                </div>
                
              </section>
              <section class="footer section-footer" v-if="authorized">
                <div class="field is-grouped" style="padding-top: 5px; margin-bottom: 0;">
                  <p class="control">
                    <figure class="image is-32x32" style="margin-right: 8px;">
                      <img :src="profile.photoURL">
                    </figure>
                  </p>
                  <p class="control is-expanded" style="margin-right: 0;">
                    <textarea class="comment-box" maxlength="250" v-model="textComment" @keyup.enter="comment" placeholder="Comment...">
                    </textarea>
                  </p>
                </div>
                <div class="field is-grouped" style="float: right;">
                  <p class="control">
                    <star-rating :increment="0.5" :star-size="20" v-model="rating" :rating='0'></star-rating>
                  </p>
                  <p class="control">
                    <a class="button is-info is-outlined send-btn" @click="comment($event)">Send</a>
                  </p>
                </div>
              </section>
              <section class="footer section-footer" v-else>
                <div class="field is-grouped" style="padding-top: 5px; margin-bottom: 0;">
                  login for comment
                </div>
              </section>
            </div>
          </div>
        </div>
        <router-link :to="'/'">
          <button class="modal-close"></button>
        </router-link>
      </div>
    </div>
    <!--Modal-->
</div>
</template>
<script>
import StarRating from 'vue-star-rating'
export default {
  props: ['id', 'posts', 'addComment', 'profile', 'login', 'authorized'],
  data () {
    return {
      count: 0,
      textComment: '',
      maxlength: 250,
      rating: 0,
      currentRating: 'No Rating',
      currentSelectedRating: 'No Current Rating',
      boundRating: 3
    }
  },
  methods: {
    comment (e) {
      var newComment = {
        photoUser: this.profile.photoURL,
        username: this.profile.displayName,
        comment: this.textComment,
        rating: this.rating
      }
      if (this.textComment === '') {
        alert('Please fill in all information.')
        e.preventDefault()
      } else {
        this.addComment(this.id, newComment)
        this.textComment = ''
      }
    },
    loginForComment () {
      this.login()
    },
    setRating (rating) {
      this.rating = 'You have Selected: ' + rating + ' stars'
    },
    showCurrentRating (rating) {
      this.currentRating = (rating === 0) ? this.currentSelectedRating : 'Click to select ' + rating + ' stars'
    },
    setCurrentSelectedRating (rating) {
      this.currentSelectedRating = 'You have Selected: ' + rating + ' stars'
    }
  },
  components: {
    StarRating
  }
}
</script>

<style lang="css">
</style>
