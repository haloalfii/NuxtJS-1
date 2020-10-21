<template>
  <section class="container">
    <div>
      <app-logo/>
      <h1 class="title">
        Perpustakaan
      </h1>
      <p colspan="4" class="title-2">Daftar Buku dan Penulis</p>
      <table border="0" rules="rows" style="margin-left=70px">
        <tr>
            <td width = "300px">
                <center>Buku</center>
            </td>
            <td width = "100px">
                Penulis
            </td>
            <td>
                <center>Action</center>
            </td>
        </tr>
        <tr>
            <td>
                <ol>
                    <li v-for="user in users" :key="user.idd"><p>{{user.buku}}</p></li>
                </ol>
            </td>
            <td>
                <p v-for="user in users" :key="user.idd">{{user.penulis}}</p>
            </td>
            <td>
                <p v-for="user in users" :key="user.idd"><button class="btn btn-outline-danger btn-sm" @click="del(user)">Delete</button></p>
            </td>
        </tr>
      </table>
      <div class="links">
        <a
          href="edit"
          class="button--green">Edit</a>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */ 
import axios from '~/plugins/axios'
import AppLogo from '~/components/AppLogo.vue'

export default {
  components: {
    AppLogo
  },

  data(){
    return{
        form: {
          id: '',
          buku: '',
          penulis:''
        },
        users: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('users').then(res => {
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('users', this.form).then(res => {
          this.load()
          this.form.buku = '',
          this.form.penulis= ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.buku = user.buku
        this.form.penulis = user.penulis 
    },
    update(form){ 
       return axios.put('users/' + form.id , {buku: this.form.buku, penulis: this.form.penulis}).then(res => {
        this.load()
        this.form.id = ''
        this.form.buku = ''
        this.form.penulis = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(user){
      axios.delete('users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.buku)
          this.users.splice(index,1)
      })
    }
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.title-2 {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 700;
  font-size: 30px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>

