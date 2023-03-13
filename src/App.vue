<template>
  <NavBar :logo="logo_src" :alt="app_name" />
  <router-view/>
  <Footer/>
 </template>

<script>
import NavBar from "./components/Navbar.vue"
import Footer from "./components/Footer.vue"

export default{
  name: "App",
  components: {
    NavBar,
    Footer
  },
  data(){
    return{
      logo_src: "/img/logo.png",
      app_name: "Barbaros Burguer"
    }
  }

}
</script>

<style>

.main-container{
  margin: 50px;
  min-height: 60vh;
}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

</style>
