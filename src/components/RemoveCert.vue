<template>
  <div class="page">
    <LoginNavBar />
    <div class="cert">
      <div id="ftr"><NavFeature /></div>
      <div class="form">
        <label>Roll-Number for Remove Cert </label>
        <div  :class="{'from-group-error': this.v$.rollNo.$error}"><input type="text" placeholder="Roll No" v-model="rollNo" /></div>
          <span class="error-feedback" v-if="this.v$.rollNo.$error">
        {{this.v$.rollNo.$errors[0].$message}}
      </span>
        <div><button v-on:click="delCert()">Delete</button></div>
      </div>
    </div>
  </div>
</template>
<script>
import useValidate from "@vuelidate/core";
import {required, minLength } from "@vuelidate/validators";
import LoginNavBar from "./LoginNavBar.vue";
import NavFeature from "./NavFeature.vue";
import axios from "axios";
export default {
  name: "RemoveCert",
  components: {
    LoginNavBar,
    NavFeature,
  },
  data() {
    return {
      v$: useValidate(),
      rollNo: "",
    };
  },
  validations(){
    return {
        rollNo : {required, minLength: minLength(8)},
    }
  },
  methods: {
    async delCert() {
       this.v$.$validate();
      if(!this.v$.$error){
        console.log("validate");
      let result = await axios.delete(
        `https://student-certificate-server.onrender.com/certs/${this.rollNo}`
      );
      if (result.status == 200) {
        alert("Certificate Deleted");
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
label{
  color: rgb(255, 255, 255);
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
