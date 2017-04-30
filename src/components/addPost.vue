<template>
  <div class="container">
    <div class="">
      <div id="image-preview">
        <input type="file" name="image" id="image-upload" @change="onFileChange"/>
        <label for="image-upload" id="image-label" >Choose File</label>
        <img id="blah" src="" alt="your image" v-show="file != ''"/>
      </div>

      <div class="field">
        <label class="label">Brand</label>
        <p class="control">
          <input class="input" type="text" placeholder="Brand" v-model="brand">
        </p>
      </div>
      <div class="field">
        <label class="label">Model</label>
        <p class="control">
          <input class="input" type="text" placeholder="Model" v-model="model">
        </p>
      </div>
      <div class="field">
        <label class="label">Message</label>
        <p class="control">
          <textarea v-model="description" rows="2" cols="80" placeholder="Someting about photo"></textarea>
        </p>
      </div>
      <div class="field is-grouped">
        <p class="control">
          <router-link to="/">
            <button class="button is-success is-outlined" @click="addPostToApp($event)">Submit</button>
          </router-link>
        </p>
      </div>
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
      brand: '',
      model: '',
      file: '',
      comments: [
        {
          comment: ' '
        }
      ],
      otherBrand: ''
    }
  },
  methods: {
    addPostToApp (e) {
      var newPost = {
        username: this.profile.displayName,
        photoUser: this.profile.photoURL,
        description: this.description,
        comments: this.comments,
        model: this.model,
        brand: this.brand
      }
      if (this.file === '' || this.model === '' || this.brand === '') {
        alert('Please fill in all information.')
        e.preventDefault()
      } else {
        this.addPost(newPost, this.file)
        this.model = ''
        this.description = ''
        this.file = ''
      }
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
<style type="text/css">
.container {
  width: 40%;
  text-align: left;
}
.container textarea {
  width: 100%;
  resize: vertical;
}
#image-preview {
  width: 400px;
  height: 400px;
  position: relative;
  overflow: hidden;
  background-color: #efefef;
  color: #ecf0f1;
  margin: auto;
}
#image-preview input {
  line-height: 400px;
  font-size: 400px;
  position: absolute;
  opacity: 0;
  z-index: 10;
}
#image-preview label {
  position: absolute;
  z-index: 5;
  opacity: 0.8;
  cursor: pointer;
  background-color: #bdc3c7;
  width: 200px;
  height: 50px;
  font-size: 20px;
  line-height: 50px;
  text-transform: uppercase;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  text-align: center;
}
</style>
