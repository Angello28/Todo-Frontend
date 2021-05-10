<template>
  <div>
    <h1>Welcome</h1>
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
  mounted: function(){
    axios
      .get('http://localhost:3000/todo')
      .then(result=>{
        this.todos = result.data
      })
  },
  methods: {
    tambah : function(){
      let newItem = {deskripsi : this.myText}
      axios
        .post('http://localhost:3000/todo', newItem)
        .then(()=>{
          axios
            .get('http://localhost:3000/todo')
            .then(result=>{
              this.todos = result.data
              this.myText = ''
            })
        })
    },
    hapus : function(id){
      axios.delete(`http://localhost:3000/todo/${id}`)
      .then(()=>{
        axios
          .get('http://localhost:3000/todo')
          .then(result=>{
            this.todos = result.data
          })
      })
    }
  }
}
</script>
