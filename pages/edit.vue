<template>
  <div>
    <p colspan="4" class="title">Daftar Buku dan Penulis</p>
    <div class="container">
      <table border="0" rules="rows">
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
                <p v-for="user in users" :key="user.idd"><button class="btn btn-outline-success btn-sm" @click="edit(user)">Edit</button> <button class="btn btn-outline-danger btn-sm" @click="del(user)">Delete</button></p>
            </td>
        </tr>
      </table>
      <fieldset class="field">
        <legend>Proses</legend>
        <table border="0">
            <tr>
                <td>Buku</td>
                <td>
                  =
                </td>
                <td><form @submit.prevent="add">
                  <input type="hidden" v-model="form.id">
                  <input type="text" class="form-control" placeholder="Judul" v-model="form.buku">
                </form></td>
            </tr>
            <tr>
                <td>Penulis</td>
                <td>
                  =
                </td>
                <td><form @submit.prevent="add">
                  <input type="text" class="form-control" placeholder="Penulis" v-model="form.penulis">
                </form></td>
            </tr>
            <tr>
              <td>
                <form @submit.prevent="add">
                  <button type="submit" class="btn btn-outline-primary btn-sm" v-show="!updateSubmit"> Add </button>  
                  <button type="button" class="btn btn-outline-primary btn-sm" v-show="updateSubmit" @click="update(form)">Update</button> 
                </form>
              </td>
            </tr>
        </table>
    </fieldset>
    </div>

    <br>
    <br>

    <div class="button-home">
        <a
          href="/"
          class="btn btn-secondary btn-sm">Back To Home</a>
      </div>
  </div>
</template>

<script>
/* eslint-disable */ 
import axios from '~/plugins/axios'
export default {
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
  min-height: 10vh;
  display: flex;
  margin-left: 70px;
}

.button-home {
  justify-content: center;
  align-items: center;
  text-align: center;
}

.field {
  justify-content: center;
  align-items: center;
  margin-left: 70px;
  margin-right: 70px;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 700;
  font-size: 30px;
  color: #35495e;
  letter-spacing: 1px;
  margin-top: 70px;
  margin-left: 70px;
}
</style>