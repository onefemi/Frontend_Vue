<template>
  <div class="about">
<h1>welcome to the future {{aboutpage}} </h1>
    <div>
        <form>
         
      <input 
        type="email"
        v-model="email"
        placeholder="user@email.com"/><br>

      <input 
        type="password"
        v-model="password"
        placeholder="Password"/><br>

      <select id="picks" @change="review">
        <option v-for="item in items" :key="item.title">{{item.title}}
        </option>
      </select><br> 

      <section>  
      <ul>
        <li v-for="post in posts" :key="post.title">{{ post.title }}</li>
      </ul>       
      </section>

      <button @click="login">dostuff</button> 
      <button id="fetchit" @click="fetchdata">Fetch Data</button> 
      <br>
      <textarea v-model="fd"></textarea>

      <h5>{{feedback}}</h5>                      

      </form>

      <form>

      <input 
        type="email"
        v-model="newuser"
        placeholder="user@email.com"/><br>

      <input 
        type="password"
        v-model="newpass"
        placeholder="Password"/><br>

      <button @click="adduser">addme</button> 

      </form>

      <table>
        <tr>
          <th v-for="post in posts" :key="post.id">{{post.id}}</th> 
        </tr>
        <tr>
          <td v-for="post in posts" :key="post.title">{{post.title}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import serviceRegister from "@/services/serviceRegister"

var data1 = {
  posts: [{
    id: 1,
    title: "Administrator"
  }, {
    id: 2,
    title: "General User"
  }]
};


export default {
  data () {
    return {
      password: "",
      email: "",
      aboutpage: "about us",
      feedback: "",
      items: [],
      fd: "",
      posts: [],
      newpass:"",
      newuser:""
    }
  },
  mounted () {
      this.items = data1.posts
  },
  methods: {
    review () {
      event.preventDefault();
      var picks = document.getElementById('picks').value
      if(picks === "General User"){
      this.fetchdata()
      }else{
        this.posts = data1.posts
      }
    },
    async login () {
      event.preventDefault();
      var response = await serviceRegister.register({
        password: this.password,
        email: this.email
      })
      this.feedback = response.data.message
    },
    fetchdata () {
        event.preventDefault();
        axios.get('https://jsonplaceholder.typicode.com/posts').then((response) => {
          this.posts = response.data
        }).catch((error) => {
          this.fd = error
        })
    },
    async adduser () {
      event.preventDefault();
      var feedUser = await serviceRegister.adduser({
       newuser: this.newuser,
       newpass: this.newpass
      })
      this.fd = feedUser.data.message
    }
  }
};
</script>

<style scoped>
input {
  margin-top: 5px;
  border-width: 1px;
  background-color: lightskyblue;
}
ul li{
 list-style-type: none;
}
</style>
