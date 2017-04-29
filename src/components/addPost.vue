<template>
  <div class="">
    Post
    <div class="">
      <img id="blah" src="" alt="your image" />
      <input type="file" @change="onFileChange"><br>
      brand
      <input type="radio" value="Nike" name="brand" @click="changeValue('Nike')" checked>
      <label for="Nike">Nike</label>
      <input type="radio" value="Adidas" name="brand" @click="changeValue('Adidas')">
      <label for="Adidas">Adidas</label><br>
      <label for="model">model</label> <input type="text" v-model="model"><br>
      Someting about photo<br>
      <textarea name="name" v-model="description" rows="8" cols="80"></textarea><br>
      <router-link to="/" ><button type="button" @click="addPostToApp">add</button></router-link><br><br><br>
    </div>
  </div>
</template>

<script>
/* global $ */
export default {
  props: ['addPost', 'profile'],
  name: 'hello',
  data () {
    return {
      description: '',
      brand: 'Nike',
      model: '',
      file: '',
      comments: [
        {
          comment: ' '
        }
      ]
    }
  },
  methods: {
    changeValue (val) {
      this.brand = val
    },
    addPostToApp () {
      var newPost = {
        username: this.profile.displayName,
        photoUser: this.profile.photoURL,
        description: this.description,
        comments: this.comments,
        model: this.model,
        brand: this.brand
      }
      this.addPost(newPost, this.file)
      this.model = ''
      this.description = ''
      this.file = ''
    },
    onFileChange (e) {
      var files = e.target.files
      this.file = files[0]
      var reader = new FileReader()
      reader.onload = function (e) {
        $('#blah').attr('src', e.target.result)
      }
      reader.readAsDataURL(this.file)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
