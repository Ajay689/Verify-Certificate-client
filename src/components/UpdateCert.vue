<template>
  <div class="page">
    <LoginNavBar />
    <div class="cert">
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
        <div><button v-on:click="updateCert()">Update</button></div>
      </div>
    </div>
  </div>
</template>
<script>
import useValidate from "@vuelidate/core";
import {required, email, minLength } from "@vuelidate/validators";
import LoginNavBar from "./LoginNavBar.vue";
import NavFeature from "./NavFeature.vue";
import axios from "axios";
export default {
  name: "UpdateCert",
  components: {
    LoginNavBar,
    NavFeature,
  },
  data() {
    return {
    v$: useValidate(),
      name: "",
      course: "",
      email: "",
      rollNo: "",
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
    async updateCert() {
       this.v$.$validate();
      if(!this.v$.$error){
        // console.log("validate");
      console.warn(this.rollNo);
      let result = await axios.patch(
        `https://student-certificate-server.onrender.com/certs/${this.rollNo}`,
        {
          name: this.name,
          course: this.course,
          email: this.email,
          rollNo: this.rollNo,
        }
      );
      console.warn(result);
      if (result.status == 200) {
        alert("Certificate Update");
      }
      localStorage.setItem("user-info", JSON.stringify(result.data));
      this.$router.push({ name: "AddCert" });

      }else{
        console.log("not validate");
      }
    },
  },
};
</script>
<style scoped>
.cert {
  display: flex;
  height: 90vh;
}
#ftr {
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
  flex-grow: 1;
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
button{
  padding: 10px;
  padding-left: 50px;
  padding-right: 50px;
}
@media only screen and (max-width: 668px) {
  .cert{
    flex-direction: column;
  }
}
</style>
