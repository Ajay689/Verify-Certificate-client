<template>
  <div id="navbar">
    <div id="deadspace"></div>
    <div>
      <img
        id="logo"
        src="https://assets.website-files.com/603683469df97967298e6e81/6037ed523cde7f1958341705_logo.png"
        alt="image"
      />
    </div>
    <div id="n_deadspace"></div>
    
    <div id="sec">
      <div class="sec">
        <router-link to="/AddCert">Welcome User</router-link>
      </div>
      <div id="deadspace"></div>
      <div id="deadspace"></div>
      <div id="deadspace"></div>
      <div id="deadspace"></div>
      <div id="deadspace"></div>
      <div class="sec"><a v-on:click="logout()" href="#">Logout</a></div>
    </div>
  </div>
</template>

<script>
export default {
  name: ":LoginNavBar",
  methods:{
        logout(){
            localStorage.clear();
            this.$router.push({name:'Public'})
        }
  }
 
}
</script>

<style scoped>
#navbar {
  display: flex;
  background: rgb(225 228 234);
  color: rgb(0, 0, 0);
  height: 60px;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
  /* border-radius: 30px;s */
  height: 46px;
}

#sec {
  display: flex;
  justify-content: center;
  align-items: center;
  
}
#deadspace{
    width: 0.5vw;
}

#logo {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 47px;
}
.sec {
  font-size: 0.8em;
}
#n_deadspace {
  width: 78vw;
}
</style>
