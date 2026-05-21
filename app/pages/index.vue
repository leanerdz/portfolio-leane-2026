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
/*  ScrollTrigger.create({
    snap: 1 / (panels.length - 1)
  });*/

});


</script>
<template>
  <!-- <section class="guide">
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
    <section class="panel profil1" id="profil1" data-theme="light">
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
      <div class="title-line"></div>
      <section class="highlighted-project">
        <!--<div class="img-project">
           <ParallaxImage :field="page?.data.highlighted_project.data.imagecouverture" />
         </div>-->
        <div class="project-image">
          <ParallaxImage :field="page?.data.highlighted_project.data.imagecouverture" />
          <!--<PrismicImage:field="page?.data.highlighted_project.data.imagecouverture"/>-->
        </div>
        <div class="highlighted-project-description">
          <div class="border">
            <h2>{{page?.data.highlighted_project.data.title}}</h2>
            <div class="info">
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
          <PrismicRichText :field="page?.data.githublink"/>
        </div>
        <div class="link">
          <PrismicImage :field="page?.data.iconinstagram"/>
          <PrismicRichText :field="page?.data.instagramlink"/>
        </div>
        <div class="link">
          <PrismicImage :field="page?.data.iconlinkedin"/>
          <PrismicRichText :field="page?.data.linkedinlink"/>
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
  position: relative;
  /*top: 70px;*/
  color: var(--color-primary);
  font-family: 'TheSeasons', serif;
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
  gap: 10%;
  margin: 0% 9.5% 0% 9.5%;
  color: var(--color-primary);
  font-family: 'Beautifully Delicious Sans', sans-serif;
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
  /*min-height: calc(100vh - 80px);
  min-height: 100vh;*/
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

/* PROJET */
.projects {
  background-color: var(--color-primary);
  padding: 3rem 1.5rem;
  font-family: 'Beautifully Delicious Sans', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.projects h1 {
  font-family: 'TheSeasons', serif;
  font-weight: normal;
  color: var(--color-secondary);
  font-size: clamp(3rem, 6vw, 5rem);
  text-align: center;
  margin: 0;
}

.title-line {
  width: min(500px, 80%);
  height: 1px;
  background: var(--color-secondary);
}

.highlighted-project {
  width: min(1100px, 100%);
  background: var(--color-secondary);
  display: grid;
  grid-template-columns: 420px 1fr;
  overflow: hidden;
}
.highlighted-project > * {
  min-width: 0;
}
.project-image {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.project-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.highlighted-project-description {
  padding: 1.5rem;
  min-width: 0;
}

.border {
  border: 1px solid var(--color-primary);
  padding: 2rem;
  height: 100% - 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 2rem;
}

.border h2 {
  text-align: center;
  color: var(--color-primary);
  font-size: clamp(2rem, 4vw, 3rem);
  margin: 0;
}

.info {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
  font-size: 1rem;
}

.border p {
  line-height: 1.6;
}

#seeMore {
  background-color: transparent;
  border: 1px solid var(--color-secondary);
  padding: 1rem 2rem;
  font-family: 'Beautifully Delicious Sans', sans-serif;
  font-size: 1rem;
  font-weight: bold;
  color: var(--color-secondary);
  cursor: pointer;
  transition:
      background-color 0.3s ease,
      color 0.3s ease;
}

#seeMore:hover {
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
  margin: 100px 0 100px 0;
}
#mail img{
  margin-right:5%;
}

.link{
  background-color: var(--color-primary);
  border: 1px solid var(--color-secondary);
  padding: 0.5rem 2rem;
  font-family: 'Beautifully Delicious Sans', sans-serif;
  font-weight: bold;
  display: flex;
  align-items: center;
  color: var(--color-secondary);
  font-size: 20px;
  justify-self: center;
  width: fit-content;
  text-wrap: nowrap;
  transition:
      background-color 0.3s ease,
      color 0.3s ease;
}

.link img{
  margin-right:5%;
}

.link:hover{
  border: 1px solid var(--color-primary);
  background-color: transparent;
  color: var(--color-primary);
}
#links-containers{
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  align-content: center;
  width: 95%;
}


@media (max-width: 500px) {
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