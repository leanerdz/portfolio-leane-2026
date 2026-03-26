<script type="text/javascript">

</script>
<template v-slot:default>
  <div id="navBar" :class="theme">
    <nav>
      <a href="">A propos de moi </a>
      <a href="">Projets</a>
      <a href="">Contact </a>
    </nav>
    <hr>
  </div>

</template>

<style scoped>
#navBar{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
}
nav{

  font-family: 'Beautifully Delicious Sans', sans-serif;
  font-weight: bolder;
  display: flex;
  justify-content: space-between;
  margin: 3% 9.5% 3% 9.5%;
}

nav a{
  color: #882A3C;
  text-decoration: none;
  font-size: 22px;
}
hr{

  height: 1px;
  width: 84.5%;
  color: #882A3C;
  background: #882A3C;
  font-size: 0;
  border: 0;
}
</style>