<script setup lang="ts">
import { asImageSrc } from "@prismicio/client";
import { components } from "~/slices";
import {gsap} from "gsap";

import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";
gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);
import NavBar from "~/components/NavBar.vue";

const { client } = usePrismic();
const { data: page } = await useAsyncData("[home]", () =>
    client.getSingle("home"),
);

useSeoMeta({
  title: page.value?.data.meta_title,
  ogTitle: page.value?.data.meta_title,
  description: page.value?.data.meta_description,
  ogDescription: page.value?.data.meta_description,
  ogImage: computed(() => asImageSrc(page.value?.data.meta_image)),
});
gsap.registerPlugin(ScrollTrigger);
onMounted(() => {
  const panels = gsap.utils.toArray(".panel");
  const setPanelHeights = () => {
    const vh = window.innerHeight;
    panels.forEach(panel => {
      panel.style.height = `${vh}px`;
    });
  };

  setPanelHeights();

  window.addEventListener('resize', setPanelHeights);
  panels.forEach((panel, i) => {
    ScrollTrigger.create({
      trigger: panel,
      start: "top top",
      pin: true,
      pinSpacing: false
    });
  });
  ScrollTrigger.create({
    snap: 1 / (panels.length - 1)
  });

});


</script>
<template>
  <!--<section class="guide">
    <div class="overlay">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
  </section>
-->
  <nav-bar/>
  <main>
    <section class="panel" id="profil1" data-theme="light">
      <h1 id="title">{{page?.data.title}}</h1>
      <div class="profil">
        <ParallaxImage :field="page?.data.photoprofil" />
        <div>
          <h2><PrismicRichText id="description_title" :field="page?.data.description_title"/></h2>
          <PrismicRichText class="description" :field="page?.data.description"/>
        </div>
      </div>
    </section>
    <section class="panel" id="profil2" data-theme="light">
      <div class="profil">
        <PrismicRichText class="description" :field="page?.data.decsription2"/>
        <ParallaxImage :field="page?.data.photopaysage" />
      </div>
    </section>
    <section class="projects panel" id="projects" data-theme="dark">
      <h1>Mes projets</h1>
      <hr>
      <section class="highlighted-project">
        <!--<div class="img-project">
          <ParallaxImage :field="page?.data.highlighted_project.data.imagecouverture" />
        </div>-->
        <PrismicImage :field="page?.data.highlighted_project.data.imagecouverture"/>
        <div class="highlighted-project-description">
          <div id="border">
            <h2>{{page?.data.highlighted_project.data.title}}</h2>
            <div id="info">
              <p>{{ page?.data.highlighted_project.data.type }}</p>
              <PrismicRichText :field="page?.data.highlighted_project.data.date"/>
            </div>
            <PrismicRichText :field="page?.data.highlighted_project.data.description"/>
          </div>
        </div>
      </section>
          <button id="seeMore">Voir plus</button>
    </section>
    <section class="footer panel" id="footer" data-theme="light">
      <h1>Me contacter</h1>
      <div id="mail">
        <PrismicImage :field="page?.data.iconmail"/>
        <PrismicRichText :field="page?.data.mail"/>
      </div>
      <div id="links-containers">
        <div class="link">
          <PrismicImage :field="page?.data.icongithub"/>
          <p>{{page?.data.github}}</p>
        </div>
        <div class="link">
          <PrismicImage :field="page?.data.iconinstagram"/>
          <p>{{page?.data.instagram}}</p>
        </div>
        <div class="link">
          <PrismicImage :field="page?.data.iconlinkedin"/>
          <p>{{page?.data.linkedin}}</p>
        </div>
      </div>
    </section>
    <SliceZone :slices="page?.data.slices ?? []" :components="components" />
  </main>
</template>
<style>

/* GUIDE */
.guide {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 9999;
}
.overlay {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
  height: 100%;
  padding: 0 24px;
}
.overlay > div {
  background-color: rgba(0, 150, 255, 0.2);
}

/*TITLE*/
#title{
  color: var(--color-primary);
  font-size: 96px;
  font-weight: normal;
  justify-self: center;
}

/*IMAGES*/


/*PROFIL*/
.profil{
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 2%;
  margin: 0% 9.5% 0% 9.5%;
  color: var(--color-primary);
  font-family: 'Beautifully Delicious Sans', serif;
  font-size: 20px;
}
.description{
  color: var(--color-primary);
}
.profil h2{
  font-size: 30px;
  text-wrap: balance;
  font-weight: normal;
}
.panel {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  align-content: center;
  justify-content: center;
  padding: 0;
  margin: 0;
}
#profil2{
  background-color: var(--color-background);
}


/*PROJECTS*/
.projects{
  background-color: var(--color-primary);
  padding-top: 40px;
  font-family: 'Beautifully Delicious Sans', serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.projects h1{
  font-family: 'TheSeasons', serif;
  font-weight: normal;
  color: var(--color-secondary);
  font-size: 64px;
  justify-self: center;
}

.projects hr{
  height: 1px;
  width: 31.3%;
  color: var(--color-secondary);
  background: var(--color-secondary);
  font-size: 0;
  border: 0;
}

.highlighted-project{
  margin-top: 40px;
  color: var(--color-primary);
  font-size: 20px;
  width: 64.5%;
  /*height: fit-content;*/
  display: flex;
  background: var(--color-secondary);
}

.highlighted-project-description{
  padding: 2%;
}
#border{
  padding: 3%;
  border: var(--color-primary) solid 1px;
  width: 93%;
  height: 93%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#border h2{
  width: 60%;
  text-align: center;
}

#info{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 3%;
  font-size: 18px;
}
#seeMore{
  background-color: var(--color-primary);
  border: 1px solid var(--color-secondary);
  margin: 3%;
  padding: 1% 2% 1% 2%;
  font-family: 'Beautifully Delicious Sans', serif;
  font-size: 20px;
  font-weight: bold;
  color: var(--color-secondary);
  transition: background-color 0.5s ease;
}
#seeMore:hover{
  background-color: var(--color-secondary);
  color: var(--color-primary);
}

/* FOOTER */
.footer{
  background-color: var(--color-background);
}
.footer h1{
  font-family: 'TheSeasons', serif;
  font-weight: normal;
  color: var(--color-primary);
  font-size: 64px;
  justify-self: center;
}

#mail{
  display: flex;
  align-items: center;
  color: var(--color-primary);
  font-size: 32px;
  justify-self: center;
}
#mail img{
  margin-right:5%;
}

.link{
  font-family: 'Beautifully Delicious Sans', serif;
  font-weight: bold;
  display: flex;
  align-items: center;
  color: var(--color-primary);
  font-size: 20px;
  justify-self: center;
  width: 25%;
}

.link img{
  margin-right:5%;
}

#links-containers{
  display: flex;
  justify-content: center;
  width: 100%;
}


@media (max-width: 800px) {
  body

  #title{
    font-size: 40px;
  }

  /*PROFIL*/
  .profil{
    gap: 2%;
    margin: 0% 9.5% 0% 9.5%;
    font-size: 15px;
    flex-direction: column;
    align-items: center;
    align-content: center;
    justify-content: center;
  }
  .profil h2{
    font-size: 20px;
  }

  .panel {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    align-content: center;
    justify-content: center;
    padding: 0;
    margin: 0;
  }

}
</style>