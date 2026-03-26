<script type="text/javascript" setup>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";
gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);
function scrollToPanel(panelId) {
  const panel = document.getElementById(panelId);
  if (panel) {
    gsap.to(window, {
      scrollTo: { y: panel, offsetY: 0 }, // scroll vers le panel
      duration: 1,
      ease: "power2.inOut"
    });
  }
}
</script>
<template v-slot:default>
  <div id="navBar">
    <nav>
      <a @click.prevent="scrollToPanel('profil1')">A propos de moi</a>
      <a @click.prevent="scrollToPanel('projects')">Projets</a>
      <a @click.prevent="scrollToPanel('footer')">Contact</a>
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