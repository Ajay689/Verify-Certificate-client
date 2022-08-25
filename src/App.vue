<template>
  <router-view></router-view>
</template>

<script>


export default {
  name: 'App',
  components: {
   
  }
}
</script>

<style>
*{
  margin:0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* background-color: #63e4b0; */
  color: #2c3e50;
}
button {
  background-color:rgba(225, 140, 95, 0.996);
  width: 100%;
  color: rgb(0, 0, 0);
  padding: 8px;
  margin: 10px 0px;
  border-radius: 15px;
  font-size: 1em;
  color: white;
  font-weight: bold;
  cursor: pointer;
}
.navbar,
#btn {
  display: flex;
  justify-content: center;
  align-items: center;
}
form {
  padding: 25px;
}
input[type="text"],input[type="password"] {
  width: 100%;
  margin: 8px 0;
  padding: 12px 20px;
  border: 2px solid rgb(17, 17, 17);
  border-radius: 18px;
  box-sizing: border-box;
}
button:hover {
  background-color: rgb(155, 68, 236);
}
.cert{
  background-color: #5c918f;
}
#fpwd {
  float: right;
  margin: 6px;
}
#signup {
  float: left;
  margin: 6px;
}
/* .login {
  margin-top: 6%;
  padding: 40px;
  /* background-color: rgb(235, 235, 235); */
  /* border-radius: 20px; */
/* } */ */
a {
  color: rgb(77, 77, 77);
}
a:link {
  text-decoration: none;
}
a:hover,
span {
  color: red;
}
.page{
  height: 100vh;
}
</style>