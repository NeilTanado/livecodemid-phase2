<template>
  <div class="container">
    <form>
      <div class="form-group">
        <label>Email address</label>
        <input type="email" class="form-control" v-model="email" placeholder="Enter email">
      </div>
      <div class="form-group">
        <label>Name</label>
        <input type="text" class="form-control" placeholder="Enter name" v-model="name">
      </div>
      <button class="btn btn-primary" @click="login">Submit</button><br>
    </form>
    <button class="btn btn-primary" @click="getImage">getItem</button>
    <div class="form-group">
      <input type="file" @change="fileName" required>
    </div>
    <button type="button" class="btn btn-outline-info" @click="postImage()">Upload Gambar</button>
  </div>
</template>

<script>
import axios from "axios"
export default {
  name: 'Home',
  data(){
    return{
      email: "",
      name: "",
      file: null,
      tampilanGambar: []
    }
  },
  methods:{
    login:function(){
      let reqToken = {
        email: this.email,
        name: this.name
      }
      axios.post('http://35.197.135.159/request-token',reqToken)
      .then((value) => {
        localStorage.setItem('token',value.data.uuid)
      })
      .catch((err) => {
        console.log(err);
      })
    },

    getImage: function(){
      axios.get('http://35.197.135.159/image',{
        headers: {Authorization : localStorage.getItem('token')}
      })
      .then((value) => {
        console.log(value);
      })
      .catch((err) => {
        console.log(err);
      })
    },

    fileName (event) {
      this.file = event.target.files[0]
    },

    postImage: function(){
      let formData = new FormData()
      formData.append('file', this.file)
      axios.post('http://35.197.135.159/image', formData ,{
        headers:{
          Authorization : localStorage.getItem('token')
        }
      })
      .then((value) => {
        console.log(value);
      })
      .catch((err) => {
        console.log(err);
      })
    },
  },
  created:{
    
  }
}
</script>

<style lang="css">
</style>



<!-- afc1ce30-6a1d-11e8-a977-9df0ed7c6516 -->
<!-- https://storage.googleapis.com/super-fox/1528354537696umaruchans.jpg -->
