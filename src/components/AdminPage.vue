<template>
<div class="page1">
<NavBar/>
  <div class="admin">
      <div>
        <!-- <img src="./girl-back.jpeg" alt="Admin-pic" id="adm-pic"> -->
        </div>
    <div class="login">
      <h2>Admin can login here</h2>
      <div :class="{'from-group-error': this.v$.email.$error}"><input type="text" placeholder="Enter Email" v-model="email" /> </div>
       <span class="error-feedback" v-if="this.v$.email.$error">
        {{this.v$.email.$errors[0].$message}} </span>
      <div :class="{'from-group-error': this.v$.password.$error}"><input type="password" placeholder="Enter Password" v-model="password" /></div>
       <span class="error-feedback" v-if="this.v$.password.$error">
        {{this.v$.password.$errors[0].$message}} </span>
      <button v-on:click="login()">LogIn</button>
    </div>
  </div>
</div>
</template>

<script>
import useValidate from "@vuelidate/core";
import {required, email, minLength } from "@vuelidate/validators";
import NavBar from "./NavBar.vue"
import axios from "axios";
export default {
  name: "LoginPage",
  components:{
    NavBar
  },
  data() {
    return {
      v$: useValidate(),
      email: "",
      password: "",
    };
  },
   validations(){
    return {
        email : {required, email },
        password : {required, minLength: minLength(6)},
      
    }
  },
  methods: {
    async login() {
        this.v$.$validate();
      if(!this.v$.$error){
        console.log("before");
       let result = await axios.post(
         `http://localhost:4000/users/login`,{email:this.email,password:this.password}
        );
       if (result.status == 200) {
         localStorage.setItem("user-info", JSON.stringify(result.data[0]));
         this.$router.push({ name:'AddCert' });
     }
       if(result.status == 401){
          alert(result.data.message);    
     }
    }
    }
  },
};
</script>

<style>
.page{
  background-color: #ffffff;
}
.admin{
  display: flex;
   justify-content: space-between;
   align-items: center; 
}
h2{
  color:rgb(94 63 220);
}
.page1{
  background-image: url("./boy-girl-bg-plus.jpg");
  background-size: cover;
  height: 100vh;
}

.from-group-error{
  color: rgb(120, 136, 225);
}
.from-group-error input{
  border-color: rgb(230, 140, 164);
} 
.error-feedback{
  color: blue;
}

  #adm-pic{
  height: 560px; 
  width: 680px;
  }
 
  .login{
   /* justify-content:center;
   align-items: center;  */
  width: 20vw;
  margin:10%;
}  
@media only screen and (max-width: 668px) {
  .admin{
    flex-direction: column;
    /* width: 80vw; */
  }
}
</style>