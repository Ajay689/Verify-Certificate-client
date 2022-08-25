<template>
<div class="page">
  <LoginNavBar/>
  <div class=cert>
    <div id="ftr"><NavFeature /></div>
    <div class="form">
      <div :class="{'from-group-error': this.v$.name.$error}"><input type="text" placeholder="Student Name" v-model="name"></div>
      <span class="error-feedback" v-if="this.v$.name.$error">
        {{this.v$.name.$errors[0].$message}}
      </span>
      <div :class="{'from-group-error': this.v$.course.$error}"><input type="text" placeholder="Course" v-model="course"></div>
      <span class="error-feedback" v-if="this.v$.course.$error">
        {{this.v$.course.$errors[0].$message}}
        </span>
      <div :class="{'from-group-error': this.v$.email.$error}"><input type="text" placeholder="Email" v-model="email"></div>
      <span class="error-feedback" v-if="this.v$.email.$error">
        {{this.v$.email.$errors[0].$message}}
        </span>
      <div :class="{'from-group-error': this.v$.rollNo.$error}"><input type="text" placeholder="Roll No" v-model="rollNo"></div>
      <span class="error-feedback" v-if="this.v$.rollNo.$error">
        {{this.v$.rollNo.$errors[0].$message}}
        </span>
      <div><button v-on:click="signup()">Add</button></div>
    </div>   
  </div>
</div>
</template>
<script scoped>
import useValidate from "@vuelidate/core";
import {required, email, minLength } from "@vuelidate/validators";
import LoginNavBar from './LoginNavBar.vue'
import NavFeature from './NavFeature.vue'
import axios from "axios";
export default {
    name:'AddCert',
    components:{
        LoginNavBar,
        NavFeature
    },
    data() {
    return {
      v$: useValidate(),
      // error: "",
        name: "",
        course: "",
        email: "",
        rollNo:"",
      
    };
  },
  validations(){
    return {
        name : {required, minLength: minLength(3)},
        course : {required, minLength: minLength(4)},
        email : {required, email },
        rollNo : {required, minLength: minLength(8)},
      
    }
  },
  methods: {
   async signup() {
      this.v$.$validate();
      if(!this.v$.$error){
        console.log("validate");
        let result = await axios.post("http://localhost:4000/certs/", 
        {
          name: this.name,
          course: this.course,
          email: this.email,
          rollNo:this.rollNo
        });
      console.warn(result);
      if (result.status == 201) {
        alert("Certificate Added");
      
      localStorage.setItem("user-info",JSON.stringify(result.data))
      localStorage.setItem("certId",result.data.cert._id)
      this.$router.push({name:'CertDownload'})
      }
      else{
        alert(result.data.message);
      }
    }
      // else{
      //   console.log("not validate");
      // }
    },
  },
  
};


</script>


<style scoped>
.cert{
    height: 90vh;
    display: flex;
}
#ftr{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-grow: 0.8;
}
.form{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  flex-grow: 0.6;
}
.from-group-error{
  color: rgb(230, 46, 46);
}
.from-group-error input{
  border-color: rgb(248, 10, 73);
} 
.error-feedback{
  color: rgb(237, 25, 25);
}
/* .error-message{
  color: red;
} */
button{
  padding: 10px;
  padding-left: 50px;
  padding-right: 50px;
}
@media only screen and (max-width: 668px) {
  .cert{
    flex-direction: column;
    /* background-color: aqua; */
  }
}
</style>