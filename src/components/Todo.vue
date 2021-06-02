<template>
  <div>
    <h1>Welcome, {{username == null ? "Anonymous" : username}}</h1>
    <div>My TO-DO List :</div>
    <ul>
      <li v-for="item in todos" :key="item.ID"><button @click="hapus(item.ID)" style="margin-right: 10px">X</button>{{item.TODO}}</li>
    </ul>
    <input v-model="myText" style="margin-right: 5px"/>
    <button @click="tambah">Add Me</button>
  </div>
</template>

<script>
import axios from 'axios'

export default{
  data : () => {
    return {
      todos:[],
      myText: ''
    }
  },
  created: function(){
    this.username = localStorage.getItem('usr')
    this.password = localStorage.getItem('pwd')
  },
  mounted: function(){
    let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
    axios
      .get('http://localhost:3000/todo', {headers})
      .then(result=>{
        this.todos = result.data
      })
  },
  methods: {
    tambah : function(){
      let newItem = {deskripsi : this.myText}
      let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
      axios
        .post('http://localhost:3000/todo', newItem, {headers})
        .then(()=>{
          axios
            .get('http://localhost:3000/todo', {headers})
            .then(result=>{
              this.todos = result.data
              this.myText = ''
            })
        })
    },
    hapus : function(id){
      let headers = {username: localStorage.getItem('usr'), password: localStorage.getItem('pwd')}
      axios.delete(`http://localhost:3000/todo/${id}`, {headers})
      .then(()=>{
        axios
          .get('http://localhost:3000/todo', {headers})
          .then(result=>{
            this.todos = result.data
          })
      })
    }
  }
}
</script>
