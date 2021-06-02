<template>
  <div>
    <h1>Login</h1>
    <div style="display: inline-block; margin-right: 10px">
        <div style="margin-bottom: 5px">Username</div><input type="text" v-model="username" style="margin-bottom: 10px"/>
    </div>
    <div style="display: inline-block; margin-right: 10px">
        <div style="margin-bottom: 5px">Password</div><input type="password" v-model="password" style="margin-bottom: 10px"/>
    </div>
    <br/>
    <button @click="simpan" style="margin-right: 5px">Save</button>
    <button @click="hapus">Delete</button>
  </div>
</template>

<script>
export default {
    data: ()=> {
        return{
            username: '',
            password: ''
        }
    },
    created: function(){
        this.username = ''
        this.password = ''
    },
    methods: {
        simpan: function(){
            localStorage.setItem('usr', this.username)
            localStorage.setItem('pwd', this.password)
            this.username = ''
            this.password = ''
        },
        hapus: function(){
            localStorage.removeItem('usr')
            localStorage.removeItem('pwd')
            this.username = ''
            this.password = ''
        }
    }
}
</script>
