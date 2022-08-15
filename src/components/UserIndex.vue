<template>
    <div>
         <div class="card" >
            <div class="card-header d-flex justify-content-between">
               <a href="#" class="btn btn-success btn-sm" @click="createUser" >Add New</a>&nbsp;
               <a href="#" class="btn btn-secondary btn-sm" @click="userList">User List</a>
            </div>
            <div class="card-body" v-if="indexArea">
                <div class="row mb-2 text-end">
                    <div class="offset-md-8 col-md-4" >
                        <input type="text" v-model="filter.search" v-on:keyup.enter="onEnterClick"  class="form-control" placeholder="Search...">
                    </div>
                </div>
                <table class="table table-bordered table-sm">
                    <thead class="bg-primary text-white">
                    <tr>
                        <th scope="col">ID</th>
                        <th scope="col">Image</th>
                        <th scope="col">First Name</th>
                        <th scope="col">Last Name</th>
                        <th scope="col">Phone</th>
                        <th scope="col">Address</th>
                        <th scope="col">Action</th>
                    </tr>
                    </thead>
                    <tbody>
                
                    <tr v-for="(user, index) in filteredUsers" :key="index">
                        <td>{{user.id}}</td>
                        <td><img :src="user.photo" style="width:80px;height:80px"/></td>
                        <td>{{user.first_name}}</td>
                        <td>{{user.last_name}}</td>
                        <td>{{user.phone}}</td>
                        <td>{{user.address}}</td>
                        <td>
                            <button class="btn btn-primary btn-sm" @click="editUser(user)">Edit</button>&nbsp;
                            <button class="btn btn-danger btn-sm" @click="deleteUser(user)">Delete</button>
                        </td>
                        
                    </tr>
                    </tbody>
                </table>
            </div>
              <div class="card-body" v-if="createArea">
                  <UserCreate></UserCreate>
              </div>
               <div class="card-body" v-if="editArea">
                  <UserEdit :edit_user="user"></UserEdit>
              </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import UserCreate from './UserCreate.vue'
import UserEdit from './UserEdit.vue'
export default {
  components: {
    UserCreate:UserCreate,
    UserEdit:UserEdit
},
    data(){
        return {
          users : [],
          user:{
            id:"",
            first_name: "",
            last_name: "",
            phone:'',
            address:'',

          },
          indexArea:true,
          createArea:false,
          editArea:false,
          filter: {
                search: ''
            },
         
        }
    },
    methods:{
       createUser(){
            this.indexArea = false;
            this.editArea=false;
            this.createArea = true;
       },
        userList(){
            this.indexArea = true;
            this.createArea = false;
            this.editArea=false;
       },
       editUser(user){
            this.indexArea = false;
            this.editArea = true;
            this.user.id = user.id;
            this.user.first_name = user.first_name;
            this.user.last_name = user.last_name;
            this.user.phone = user.phone;
            this.user.address = user.address;
       },
       deleteUser(user){
                const formsData = new FormData();
                formsData.append('_method', 'DELETE');
                axios.post(`http://127.0.0.1:8000/api/users/${user.id}`, formsData,{headers: {
                        'Content-Type': 'multipart/form-data'
                    }}).then((response) => {
                         alert(response.data);
                        window.location.reload();
                    });
       }
       
    },
    computed: {
        filteredUsers: function () {
            if (this.filter.search != '') {
                return this.users.filter((user) => {
                    return user.first_name.toLowerCase().match(this.filter.search.toLowerCase())
                           ||user.last_name.toLowerCase().match(this.filter.search.toLowerCase())
                           ||user.phone.toLowerCase().match(this.filter.search.toLowerCase())
                           ||user.address.toLowerCase().match(this.filter.search.toLowerCase());
                          

                });
            }

            return this.users;
        },
    },
    beforeMount(){
        axios.get('http://127.0.0.1:8000/api/users').then((response) => {
            this.users = response.data.data;
            console.log( this.users)
          
        });
    }
}
</script>