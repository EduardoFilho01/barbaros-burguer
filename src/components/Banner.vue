<template>
  <div id="main-banner">
    <h1> <em>Bárbaros Burguer</em> </h1>
  </div>
</template>

<script>
export default{
  name: "Banner"

}
</script>


<style scoped>

#main-banner {
  background-image: url('/public/img/burger.jpg');
  background-position: 0 -250px;
  background-size: cover;
  height: 800px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#main-banner h1{
  text-align: center;
  font-size: 60px;
  color: #fff;
  background-color: #222;
  padding: 20px 40px;
}

</style>