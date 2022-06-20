<template>
  <div id="app">

    <transition name="slide-fade">
      <FlashBox v-show="showflash" v-bind:flashMsg="flashMsg" v-on:showFlashBox="updateFlashBoxStatus"/>
    </transition> 

    <h1 class="text-center">{{title}}</h1>

    <div class="d-flex justify-content-start my-3">
        <button type="button" class="btn btn-primary" v-on:click="showCreateModal">Add New User</button>
    </div>

    <transition name="fade">
      <CreateForm v-show="showcreate" v-on:modalStatusChanged="updateCreateModalStatus"/>
    </transition>

    <transition name="fade">
      <EditForm  v-show="showedit" v-on:modalStatusChanged="updateEditModalStatus" v-bind:selectedUser="selectedUser"/>
    </transition>

    <UserList>
      <tr v-for="user in users" v-bind:key="user.id">
          <td> {{ user.id }} </td>
          <td> {{ user.name }} </td>
          <td> {{ user.emails[0] }} </td>
          <td class="d-flex justify-content-around">
              <button type="button" class="btn btn-sm btn-warning" v-on:click="showEditModal(user.id)">Edit</button> 
              <button type="button" class="btn btn-sm btn-danger" v-on:click="deleteUser(user.id)">Delete</button>
          </td>
      </tr>
    </UserList>

  </div>
</template>

<script>
import UserList from './components/UserList.vue'
import CreateForm from './components/CreateForm.vue'
import EditForm from './components/EditForm.vue'
import FlashBox from './components/FlashBox.vue'

export default {
  name: 'App',
  data: ()=>{
      return {
        title: "Vue Project - Xiaojing Shi",
        users: [],
        uid: '',
        selectedUser: [],
        showcreate: false,
        showedit: false,
        showflash: false,
        flashMsg: ''
      }
  },
  components: {
    UserList,
    CreateForm,
    EditForm,
    FlashBox
  },
  methods: {
    getUsers(){
        fetch('http://localhost:8000/users')
        .then(response => response.json())
        .then(json => {
            console.log(json.body);
            this.users = json.body.reverse();
        })
        .catch(err => {
            console.log(err);
        })
    },

    getOneUser(){
        fetch('http://localhost:8000/users?id=' + this.uid)
        .then(response => response.json())
        .then(json => {
            console.log(json.body);
            this.selectedUser = json.body;
        })
        .catch(err => {
            console.log(err);
        })
    },
    
    getOneUserWithCallback(callback){
        fetch('http://localhost:8000/users?id=' + this.uid)
        .then(response => response.json())
        .then(json => {
            console.log(json.body);
            this.selectedUser = json.body;
            callback();
        })
        .catch(err => {
            console.log(err);
        })
    },

    deleteUser(id){
      if(confirm("Are you sure to delete this user?")){
        this.uid = id;
        this.getOneUserWithCallback(() => {
          fetch('http://localhost:8000/users', {
              method: 'delete',
              headers: {
                  'Content-Type' : 'application/json'
              },
              body: JSON.stringify(this.selectedUser) //to send it as a string
          })
          .then(response => response.json())
          .then(json => {
              console.log(json.body);
              // reload user list
              this.getUsers();
              // flash message
              this.flashMsg = "User has been successfully deleted";
              this.showflash = true;
              setTimeout(()=>{
                this.showflash = false;
              },2000);
              
          })
          .catch(err => {
              console.log(err);
          })
        });
      }  
    },

    showCreateModal(){
      this.showcreate = true
    },

    showEditModal(id){
      this.uid = id;
      this.getOneUser();
      this.showedit = true
    },

    updateCreateModalStatus(e){
      this.showcreate = e;
      this.getUsers();
      // flash message
      this.flashMsg = "User has been successfully added";
      this.showflash = true;
      setTimeout(()=>{
        this.showflash = false;
      },2000);

    },

    updateEditModalStatus(e){
      this.showedit = e;
      this.getUsers();
      // flash message
      this.flashMsg = "User has been successfully updated";
      this.showflash = true;
      setTimeout(()=>{
        this.showflash = false;
      },2000);
    }

  },

  created(){
      this.getUsers();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  width: 60%;
  margin: 0 auto;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.6s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.slide-fade-enter-active, .slide-fade-leave-active{
  transition: all 0.8s ease;
}
.slide-fade-enter {
  opacity: 0;
}
.slide-fade-leave-to {
  transform: translateY(-50px);
  opacity: 0;
}
</style>