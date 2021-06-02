<template>
  <div>
    <h1>Welcome, {{username == null ? "Anonymous" : username}}</h1>
    <div style="display: inline-block; margin-right: 10px">
        <div style="margin-bottom: 5px">Username</div><input v-model="username" style="margin-bottom: 10px"/>
    </div>
    <div style="display: inline-block; margin-right: 10px">
        <div style="margin-bottom: 5px">Password</div><input v-model="password" type="password" style="margin-bottom: 10px"/>
    </div>
    <button @click="tambah" style="margin-bottom: 20px">Add</button>
    <div>My User's List :</div>
    <ul>
      <li v-for="item in users" :key="item.ID"><button @click="hapus(item.ID)" style="margin-right: 10px">X</button>{{item.USERNAME}}</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default{
  data : () => {
    return {
      users: [],
      username: '',
      password: ''
    }
  },
  created: function(){
    this.username = localStorage.getItem('usr')
    this.password = localStorage.getItem('pwd')
  },
  mounted: function(){
    let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
    axios
      .get('http://localhost:3000/user', {headers})
      .then(result=>{
        this.users = result.data
      })
  },
  methods: {
    tambah : function(){
      let newItem = {username: this.username, password: this.password}
      let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
      axios
        .post('http://localhost:3000/user', newItem, {headers})
        .then(()=>{
          axios
            .get('http://localhost:3000/user', {headers})
            .then(result=>{
              this.users = result.data
              this.username = ''
              this.password = ''
            })
        })
    },
    hapus : function(id){
      let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
      axios.delete(`http://localhost:3000/user/${id}`, {headers})
      .then(()=>{
        axios
          .get('http://localhost:3000/user', {headers})
          .then(result=>{
            this.users = result.data
          })
      })
    }
  }
}
</script>
