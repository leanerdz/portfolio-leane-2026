<script setup lang="ts">
import { asImageSrc } from "@prismicio/client";
import { components } from "~/slices";
import {gsap} from "gsap";

import { ScrollTrigger } from "gsap/ScrollTrigger";
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

  panels.forEach((panel, i) => {
    ScrollTrigger.create({
      trigger: panel,
      start: "top top",
      pin: true,
      pinSpacing: true
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
        <PrismicImage :field="page?.data.photoprofil"/>
        <PrismicRichText class="description" :field="page?.data.description"/>
      </div>
    </section>
    <section class="panel" id="profil2" data-theme="light">
      <div class="profil">
        <PrismicRichText class="description" :field="page?.data.decsription2"/>
        <PrismicImage :field="page?.data.photopaysage"/>
      </div>
    </section>
    <section class="projects panel" data-theme="dark">
      <h1>Mes projets</h1>
      <hr>
      <section class="highlighted-project">
        <PrismicImage :field="page?.data.highlighted_project.data.imagecouverture"/>
        <!--<img id="" src="../images/STV/STV1.png" alt="Image de présentation de Léane Rodriguez">-->
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
    <section class="footer panel" data-theme="light">
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
    <!--
    <div class="squareList">
      <div class="square"></div>
      <div class="square"></div>
      <div class="square"></div>
      <div class="square"></div>
      <div class="square"></div>
    </div>
    -->
    <SliceZone :slices="page?.data.slices ?? []" :components="components" />
  </main>
</template>
<style>
/* FONTS */
@font-face {
  font-family: 'BeautifullyDelicious';
  src: url('../fonts/Beautifully_Delicious_Font/BDSans-Regular.otf') format('otf');
  font-weight: normal;
  font-style: normal;
}
@font-face {
  font-family: 'TheSeasons';
  src: url('../fonts/The_Seasons_Font/Fontspring-DEMO-theseasons-bd.otf') format('otf');
  font-weight: normal;
  font-style: normal;
}

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

/* BODY */
body{
  background-color: #FAF0EE;
  margin: 0;
  padding: 0;
}

/*TITLE*/
h1{
  font-family: 'TheSeasons', serif;
}
#title{
  color: #882A3C;
  font-size: 96px;
  font-weight: normal;
  justify-self: center;
}

/*IMAGES*/

/*PROFIL 1*/
.profil{
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 2%;
  margin: 0% 9.5% 0% 9.5%;
  color: #882A3C;
  font-family: 'Beautifully Delicious Sans', serif;
  font-size: 20px;
}
.description{
  color: #882A3C;
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

/*PROJECTS*/
.projects{
  background-color: #882A3C;
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
  color: #FEE9E6;
  font-size: 64px;
  justify-self: center;
}

.projects hr{
  height: 1px;
  width: 31.3%;
  color: #FEE9E6;
  background: #FEE9E6;
  font-size: 0;
  border: 0;
}

.highlighted-project{
  margin-top: 40px;
  color: #882A3C;
  font-size: 20px;
  width: 64.5%;
  /*height: fit-content;*/
  display: flex;
  background: #FEE9E6;
}

.highlighted-project-description{
  padding: 2%;
}
#border{
  padding: 3%;
  border: #882A3C solid 1px;
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
  margin: 3%;
  padding: 1% 2% 1% 2%;
  font-family: 'Beautifully Delicious Sans', serif;
  font-size: 20px;
  font-weight: bold;
  color: #882A3C;
}
.footer{
  /*margin: 10% 0% 10% 0%;*/
}
.footer h1{
  font-family: 'TheSeasons', serif;
  font-weight: normal;
  color: #882A3C;
  font-size: 64px;
  justify-self: center;
}

#mail{
  display: flex;
  align-items: center;
  color: #882A3C;
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
  color: #882A3C;
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
/*
.square {
  width: 50px;
  height: 50px;
  background-color: brown;
}
.squareList {
  margin-top: 50vh;
  margin-bottom: 50vh;
}*/
</style>