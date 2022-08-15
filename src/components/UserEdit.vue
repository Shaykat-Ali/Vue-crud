<template>
    <div>
      <div class="card  col-md-8 offset-md-2 text-start">
      <div class="card-header text-white bg-primary">Edit User</div>
      <div class="card-body">
        <form @submit.prevent="updateUser" enctype="multipart/form-data">
            <div class="row">
                <div class="form-group col-md-6 mt-3">
                    <input type="text" v-model="singleUser.first_name" class="form-control" placeholder="Enter First Name" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="text" v-model="singleUser.last_name" class="form-control" placeholder="Enter Last Name" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="number" v-model="singleUser.phone" class="form-control" placeholder="Enter Phone" required>
                </div>
                <div class="form-group col-md-6 mt-3">
                    <input type="file"  class="form-control" placeholder="Choose File" @change="uploadImage">
                </div>
                 <div class="form-group col-md-12 mt-3">
                    <textarea  class="form-control" placeholder="Enter Address" v-model="singleUser.address"></textarea>
                </div>
                 <button type="submit" class="btn btn-primary mt-3"><Strong>Update</Strong></button>
            </div>
        </form>
        </div>
      </div>
    </div>
</template>
<script>
import axios from 'axios';
export default {
   props:{
      edit_user :Object
   },
   data(){
        return {
          singleUser:{
             id:this.edit_user.id,
             first_name: this.edit_user.first_name,
             last_name: this.edit_user.last_name,
             phone: this.edit_user.phone,
             address: this.edit_user.address,
             photo: ""
          },
          indexArea:false,
          createArea:false,
          editArea:true,
        }
    }, 
    methods:{
        userList(){
            this.indexArea = true;
            this.createArea = false;
            this.editArea=false;
       },
        uploadImage(e) {
            this.singleUser.photo = e.target.files[0];
           console.log( this.singleUser.photo);
        },
        updateUser() {
            const formsData = new FormData();
                    formsData.append('photo', this.singleUser.photo);
                    formsData.append('first_name', this.singleUser.first_name);
                    formsData.append('last_name', this.singleUser.last_name);
                    formsData.append('phone', this.singleUser.phone);
                    formsData.append('address', this.singleUser.address);
                    formsData.append('_method', 'PATCH');
                    axios.post(`http://127.0.0.1:8000/api/users/${this.singleUser.id}`, formsData,{headers: {
                        'Content-Type': 'multipart/form-data'
                    }}).then((response) => {
                        console.log(response.data);
                         alert(response.data);
                        window.location.reload();
                    });
        },
      
    },
}
</script>