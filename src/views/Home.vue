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
      <button class="btn btn-danger mb-3 ml-3 mr-3 mb-3" @click="login">Get Your Token First!</button><br>
    </form>
    <div class="form-group mb-3 ml-3 mr-3 mb-3">
      <input type="file" @change="fileName" required>
    </div>
    <button type="button" class="btn btn-outline-info" @click="postImage()">Upload Gambar</button><br>
    <button class="btn btn-primary mt-3" @click="getImage">getItem</button>
    <div class="container2">
     <div class="card mt-4 ml-4 mb-4 mr-4" style="width: 18rem;" v-for = "image in images">
        <img class="card-img-top" img :src=image.url alt="Card image cap">
        <div class="card-body">
          <h5 class="card-title">{{image.user.name}}</h5>
        </div>
      </div>
    </div>
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
      images: []
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
      let self = this
      axios.get('http://35.197.135.159/image',{
        headers: {Authorization : localStorage.getItem('token')}
      })
      .then((value) => {
        console.log(value.data);
        self.images = value.data
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
}
</script>

<style lang="css">
.container2{
  display: flex;
  flex-wrap: wrap-reverse;
  flex-direction: row  | column ;
  justify-content: center;
}
</style>
