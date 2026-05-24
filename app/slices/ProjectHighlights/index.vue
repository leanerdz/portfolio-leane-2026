<script setup lang="ts">
import type { Content } from "@prismicio/client";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";


const props = defineProps(getSliceComponentProps<Content.ProjectHighlightsSlice>());

gsap.registerPlugin(ScrollTrigger);

const project = ref<HTMLElement | null>(null);


let tl: gsap.core.Timeline;

/*onMounted(() => {
  if (!project.value) return;

  tl = gsap.timeline({
    scrollTrigger: {
      scroller: ".scrolling",
      trigger: project.value,
      start: "top 10%",
      end: "+=500",
      scrub: true,
      pin: true,
      markers: true,
    }
  });

  tl
      .fromTo(
          project.value,
          { opacity: 0 },
          { opacity: 1, ease: "none", duration: 1 }
      )
      .to(project.value, {
        opacity: 1,
        ease: "none",
        duration: 1,
      })
      .to(project.value, {
        opacity: 0,
        ease: "none",
        duration: 1,
      });
});

onUnmounted(() => {
  tl?.scrollTrigger?.kill();
  tl?.kill();
});*/

</script>
<template>
  <section
    ref="project"
    :data-slice-type="slice.slice_type"
    :data-slice-variation="slice.variation"
    class="project"
    :class="slice.primary.layout"
  >
    <div class="project_gallery">
      <PrismicImage id="img1" :field="slice.primary.project_images[0].image1"/>
      <div id="line2">
        <div class="line">
          <PrismicImage :field="slice.primary.project_images[1].image1"/>
        </div>
        <div class="line">
          <PrismicImage :field="slice.primary.project_images[2].image1"/>
        </div>
      </div>
    </div>
    <div class="project_content">
      <div class="border">
        <h2><PrismicRichText :field="slice.primary.title"/></h2>
        <p id="details"><PrismicRichText :field="slice.primary.subtitle"/> <b><PrismicRichText :field="slice.primary.date"/></b></p>
        <p>En collaboration avec <span v-for="person in slice.primary.collaborators" :key="person.name" ><b>{{ person.name }}, </b> </span> </p>
        <PrismicRichText :field="slice.primary.description"/>
        <p>Avec <span v-for="tech in slice.primary.tech" :key="tech.tech_name" ><b>{{ tech.tech_name }}, </b> </span> </p>

      </div>
    </div>
  </section>
</template>
<style scoped>
/*body{
  overflow: hidden;
}*/
.project {
  width: 75%;
  background: var(--color-secondary);
  display: grid;
  grid-template-columns: 520px 1fr;
  overflow: hidden;
  font-family: "Beautifully Delicious Sans", sans-serif;
  color: var(--color-primary);
  margin: auto auto 5rem auto;
  will-change: transform;
  box-sizing:border-box;
  align-items:stretch;
}

.default {
  margin-left: 5%;
}
.reverse {
  direction: rtl;
  margin-right: 5%;

}

.project.reverse * {
  direction: ltr;
}
.project_gallery{
  width:100%;
  overflow:hidden;
  display:flex;
  flex-direction:column;
}

#img1{
  width:100%;
  height:auto;
  overflow:hidden;
  display:block;
}

#img1 :deep(img){
  width:100%;
  height:100%;
  object-fit:cover;
  display:block;
}
#line2 {
  display:flex;
  flex:1;
}
.line{
  flex:1;
  overflow:hidden;
  display:flex;
}
.line :deep(img) {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.project_content{
  padding:1rem;
  display:flex;
  min-width:0;
  box-sizing:border-box;
}
.border{
  border:1px solid var(--color-primary);
  padding:2rem;
  display:flex;
  flex-direction:column;
  justify-content:center;
  gap:.75rem;
  box-sizing:border-box;
  width:100%;
  height:100%;
}
.border h2 {
  text-align: center;
  color: var(--color-primary);
  font-size: 35px;
  margin: 0;
}
.border p {
  line-height: 1.6;
  margin: 0;
}

#details {
  display: flex;
  gap: 10px;
}

@media (max-width:768px){

  .project{
    width:92%;
    display:flex;
    flex-direction:column;
    margin:0 auto 3rem;
  }

  .default,
  .reverse{
    margin-left:auto;
    margin-right:auto;
  }

  .reverse{
    direction:ltr;
  }

  .project_gallery{
    width:100%;
  }

  #img1{
    width:100%;
    display:block;
  }

  #line2{
    display:flex;
  }

  .line{
    aspect-ratio:1/1;
  }

  .project_content{
    padding:1rem;
  }

  .border{
    padding:1.25rem;
  }

  .border h2{
    font-size:26px;
    text-align:center;
  }

  .border p{
    font-size:15px;
    line-height:1.5;
  }

  #details{
    flex-direction:column;
    gap:4px;
  }
}
</style>