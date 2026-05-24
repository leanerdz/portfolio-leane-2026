<script setup lang="ts">
import { asImageSrc } from "@prismicio/client";
import { components } from "~/slices";
import {gsap} from "gsap";

import ParallaxImage from "~/components/ParallaxImage.vue";
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

let triggers: ScrollTrigger[] = [];
onMounted(() => {
  const panels = gsap.utils.toArray(".panel");

  const mm = gsap.matchMedia();

  mm.add("(min-width: 769px)", () => {
    const setPanelHeights = () => {
      const vh = window.innerHeight;
      panels.forEach((panel: any) => {
        panel.style.height = `${vh}px`;
      });
    };

    setPanelHeights();
    window.addEventListener("resize", setPanelHeights);

    triggers = panels.map((panel: any) =>
        ScrollTrigger.create({
          trigger: panel,
          start: "top top",
          pin: true,
          pinSpacing: false,
        })
    );

    ScrollTrigger.refresh();

    return () => {
      window.removeEventListener("resize", setPanelHeights);
      triggers.forEach(t => t.kill());
    };
  });

  mm.add("(max-width: 768px)", () => {
    panels.forEach((panel: any) => {
      panel.style.height = "auto";
    });
  });
});

onUnmounted(() => {
  window.removeEventListener("resize", setPanelHeights);

  triggers.forEach(t => t.kill());
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
        <ParallaxImage :field="page?.data.photoprofil"/>
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
          <NuxtLink id="seeMore" to="/ProjectGallery">Voir plus</NuxtLink>
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
  gap: 10%;
  margin: 0% 9.5% 0% 9.5%;
  color: var(--color-primary);
  font-family: 'Beautifully Delicious Sans', sans-serif;
  font-size: 20px;
  align-items: center;
  height: auto;
}

.profil img {
  width: 100%;
  height: auto;
  display: block;
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
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  box-sizing:border-box;
  gap: 2rem;
  color: var(--color-primary);
}

.border h2 {
  text-align: center;
  color: var(--color-primary);
  font-size: 35px;
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


@media (max-width:768px){

  .panel{
    min-height:auto;
    height:auto !important;
    padding:80px 0;
    justify-content:flex-start;
  }

  #title{
    font-size:56px;
    text-align:center;
    line-height:1.1;
    padding:0 20px;
    margin-bottom:40px;
  }

  .profil{
    flex-direction:column;
    gap:40px;
    margin:0 24px;
    font-size:17px;
    text-align:center;
  }

  .profil h2{
    font-size:24px;
  }

  .profil img{
    width:100%;
    max-width:420px;
    height:auto;
  }

  .projects{
    padding:80px 24px;
    gap:32px;
  }

  .projects h1{
    font-size:48px;
  }

  .highlighted-project{
    display:flex;
    flex-direction:column;
    width:100%;
  }

  .project-image{
    height:320px;
  }

  .project-image img{
    width:100%;
    height:100%;
    object-fit:cover;
  }

  .highlighted-project-description{
    padding:1rem;
  }

  .border{
    padding:1.25rem;
    gap:1rem;
  }

  .border h2{
    font-size:28px;
    text-align:center;
  }

  .info{
    flex-direction:column;
    gap:.25rem;
    text-align:center;
  }

  #seeMore{
    width:100%;
    max-width:280px;
    text-align:center;
  }

  .footer{
    padding:80px 24px;
  }

  .footer h1{
    font-size:48px;
    text-align:center;
  }

  #mail{
    flex-direction:column;
    gap:16px;
    font-size:22px;
    text-align:center;
    margin:60px 0;
  }

  #mail img{
    margin-right:0;
    width:40px;
  }

  #links-containers{
    flex-direction:column;
    gap:20px;
    width:100%;
  }

  .link{
    width:100%;
    justify-content:center;
    font-size:18px;
    padding:1rem;
  }

  .link img{
    width:24px;
  }
}
</style>