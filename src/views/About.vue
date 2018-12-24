<template>
  <div class="about">
<h1>welcome to the future {{aboutpage}} </h1>
    <div>
     
        <input 
        type="text"
        v-model="password"
        placeholder="password"/><br>

        <input 
        type="email"
        v-model="email"
        placeholder="email"/><br/>

      <section>  
      <ul>
        <li v-for="post in posts">{{ post.title }}</li>
      </ul>       
      </section>

      <table>
        <tr>
          <th v-for="post in posts">{{post.id}}</th> 
        </tr>
        <tr>
          <td v-for="post in posts">{{post.title}}</td>
        </tr>
      </table>

      <button @click="doit">doit</button> 
      <button @click="testz">axiosit</button> 
      <br>
      <textarea v-model="fd"></textarea>

      <h5>{{feedback}}</h5>                      
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
    title: "My Post One"
  }, {
    id: 2,
    title: "My Post Two"
  }]
};


export default {
  data () {
    return {
      password: " ",
      email: " ",
      aboutpage: "about us",
      feedback: "",
      fd: [],
      posts: []
    };
  },
  methods: {
    async doit () {
      var response = await serviceRegister.register({
        password: this.password,
        email: this.email
      });
      this.feedback = response.data.message
    },
    testz () {
        axios.get('https://jsonplaceholder.typicode.com/posts').then((response) => {
          this.posts = response.data
          this.fd = response.data[0].title               
          console.log(this.posts)
        }).catch((error) => {
          console.log(error)
        })
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
</style>
