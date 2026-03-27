<script type="text/javascript" setup>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";
import { useIntersectionObserver } from '@vueuse/core'
import { shallowRef, useTemplateRef } from 'vue'
import { onMounted, ref } from 'vue'

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);
function scrollToPanel(panelId) {
  const panel = document.getElementById(panelId);
  if (panel) {
    gsap.to(window, {
      scrollTo: { y: panel, offsetY: 0 },
      duration: 1,
      ease: "power2.inOut"
    });
  }
}

const currentTheme = ref("light")

onMounted(() => {
  const sections = document.querySelectorAll(".panel")

  sections.forEach((section) => {
    ScrollTrigger.create({
      trigger: section,
      start: "top top+=80",
      end: "bottom top+=80",
      onEnter: () => {
        currentTheme.value = section.getAttribute("data-theme") || "light"
      },
      onEnterBack: () => {
        currentTheme.value = section.getAttribute("data-theme") || "light"
      }
    })
  })
})

const colorChange = () => {
  if (currentTheme.value === "dark") {
    return "var(--color-secondary)"
  }
  else return "var(--color-primary)"
}

</script>
<template v-slot:default>
  <div id="navBar" >
    <nav :style="{ color: colorChange() }">
      <a @click.prevent="scrollToPanel('profil1')" >A propos de moi</a>
      <a @click.prevent="scrollToPanel('projects')">Projets</a>
      <a @click.prevent="scrollToPanel('footer')">Contact</a>
    </nav>
    <hr :style="{ backgroundColor: colorChange() }" />
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
}

nav a{
  text-decoration: none;
  font-size: 22px;
  transition: transform 0.2s ease, opacity 0.2s ease;
  margin: 2% 9.5% 1.5% 9.5%;
}
nav a:hover {
  transform: translateY(-2px);
  opacity: 0.7;
}

hr{
  height: 1px;
  width: 84.5%;
  font-size: 0;
  border: 0;
}

@media (max-width: 500px) {

  nav a{
    font-size: 13px;
  }

}
</style>