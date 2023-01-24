<template>
  <div class="page">
    <NavBar />
    <!-- <img src="./student-group-with-tham.jpeg" alt=""> -->
    <div class="public">
      <div id="student-pic">
        <!-- <img src="./boy-standing-with-lap.jpeg" alt="student-pic" id="in-student-pic" /> -->
        <!-- <img src="./student-with-tham.jpeg" alt="student-pic" id="in-student-pic" /> -->
      </div>
      <div id="public_search">
        <div class="search">
          <h2 >Student can Verify Certificates Here</h2>
          <div  :class="{'from-group-error': this.v$.certId.$error}">
            <input
              id="CertificateId"
              type="text"
              placeholder="Enter Certificate id"
              name="cert_id"
              v-model="certId"
              required
            />
          </div>
           <span class="error-feedback" v-if="this.v$.certId.$error">
        {{this.v$.certId.$errors[0].$message}}
      </span>
          <div id="btn">
            <button v-on:click="search()" id="submit" type="submit">
              Verify
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import useValidate from "@vuelidate/core";
import {required, minLength } from "@vuelidate/validators";
import NavBar from "../components/NavBar.vue";
import axios from "axios";
export default {
  name: "PublicPage",
  components: {
    NavBar,
  },
  data() {
    return {
      // images:[],
      v$: useValidate(),
      certId: "",
    };
  },
   validations(){
    return {
        certId : {required, minLength: minLength(24)},
    }
  },
  methods: {
    async search() {
       this.v$.$validate();
      if(!this.v$.$error){
        console.log("validate");
      let result = await axios.get(
        `https://student-certificate-server.onrender.com/certs/${this.certId}`
        // `https://evening-inlet-19531.herokuapp.com/certs/${this.certId}`
      );
        // console.log("hii");
      if (result.status == 200) {
         localStorage.clear();
         localStorage.setItem("certId", result.data.cert[0]._id);
        this.$router.push("/verify");
      }

      if (result.status == 204) {
        alert(result.statusText);
       }
      }
    },
  },
};
</script>
<style scoped>
.page {
  background-color: #ffff;
}
 .page{
  background-image: url("./bg1-plus.jpg");
  background-size: cover;
} 
.public {
  height: 90vh;
  display: flex;
   /* justify-content: center; */
  align-items: center;
}
#student-pic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60%;
  height: 540px;
  margin-top: 40px;
}
#in-student-pic {
  height: 510px;
  width: 721px;
  border-radius: 250px;
  margin: 21px;
}
.from-group-error{
  color: rgb(249, 50, 43);
}
.from-group-error input{
  border-color: rgb(235, 54, 54);
} 
.error-feedback{
  color: rgb(241, 30, 30);
}

#public_search {
   display: flex; 
   justify-content: center;
  align-items: center; 
  margin-left: 50px;
}
h2{
  color: rgb(4,4,4);
}
@media only screen and (max-width: 668px) {
 .public {
    flex-direction: column;
  }
 #public_search{
   width: 80vw; 
 }
    }

</style>
