<template>
    <div>
      <div class="card  col-md-8 offset-md-2 text-start">
      <div class="card-header text-white bg-primary"> Add User</div>
      <div class="card-body">
        <form @submit.prevent="storeUser" enctype="multipart/form-data">
            <div class="row">
                <div class="form-group col-md-6 mt-3">
                    <input type="text" v-model="user.first_name" class="form-control" placeholder="Enter First Name" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="text" v-model="user.last_name" class="form-control" placeholder="Enter Last Name" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="number" v-model="user.phone" class="form-control" placeholder="Enter Phone" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="file"  class="form-control" placeholder="Choose File" @change="uploadImage">
                </div>
                 <div class="form-group col-md-12 mt-3">
                    <textarea  class="form-control" placeholder="Enter Address" v-model="user.address"></textarea>
                </div>
                 <button type="submit" class="btn btn-primary mt-3"><Strong>Submit</Strong></button>
            </div>
        </form>
        </div>
      </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
   data(){
        return {
          users : [],
          user:{
            id:"",
            first_name: "",
            last_name: "",
            phone:'',
            address:'',
            photo:''
          },
          indexArea:false,
          createArea:true
        }
    }, 
    methods:{
       userList(){
            this.indexArea = true;
            this.createArea = false;
       },
        uploadImage(e) {
            this.user.photo = e.target.files[0];
           console.log( this.user.photo);
        },
        storeUser() {
            const formData = new FormData();
                    formData.append('photo', this.user.photo);
                    formData.append('first_name', this.user.first_name);
                    formData.append('last_name', this.user.last_name);
                    formData.append('phone', this.user.phone);
                        formData.append('address', this.user.address);

                    axios.post(`http://127.0.0.1:8000/api/users`, formData,{headers: {
                        'Content-Type': 'multipart/form-data'
                    }}).then((response) => {
                        console.log(response);
                        alert(response.data);
                        window.location.reload();
                    });
        },
      
    },
}
</script>