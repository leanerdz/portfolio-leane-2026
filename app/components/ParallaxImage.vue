<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

defineProps<{
  field: any;
}>();

const el = ref<HTMLElement | null>(null);

let tween: gsap.core.Tween | null = null;

onMounted(() => {
  const yStart = 100;
  const yEnd = -100;
  const distance = Math.abs(yEnd - yStart);
  const initialScale = 1 + distance * 0.3 / distance;


  if (!el.value) return;

  const img = el.value.querySelector("img");

  if (!img) return;

  gsap.set(img, {
    y: 100,
    scale: initialScale
  });

  gsap.set(img, {scale: 1.2});

  tween = gsap.to(img, {
    y: -100,
    ease: "none",

    scrollTrigger: {
      trigger: el.value,
      start: "top bottom",
      end: "bottom top",
      scrub: true,
    },
  });

  img.onload = () => {
    ScrollTrigger.refresh();
  };
});

onBeforeUnmount(() => {
  tween?.scrollTrigger?.kill();
  tween?.kill();
});
</script>

<template>
  <div class="parallax-wrapper" ref="el">
    <PrismicImage
        :field="field"
        class="parallax-img"
    />
  </div>
</template>

<style scoped>
.parallax-wrapper {
  overflow: hidden;
  width: 100%;
  height: 100%;
}

.parallax-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;

  will-change: transform;
}
</style>




<!--<script setup lang="ts">
import { ref, onMounted } from "vue";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { PrismicImage } from "@prismicio/vue";

gsap.registerPlugin(ScrollTrigger);

const props = defineProps<{
  field: any;
}>();

const el = ref<HTMLElement | null>(null);

onMounted(() => {
  if (!el.value) return;
  const yStart = 100;
  const yEnd = -100;
  const distance = Math.abs(yEnd - yStart);

  const initialScale = 1 + distance * 0.2 / distance;

  gsap.set(".parallax-img", { y: yStart, scale: initialScale });

    gsap.set(".parallax-img", {scale: 1.2});
    gsap.fromTo(
        ".parallax-img",
        { y: yStart },
        {
          y: yEnd,
          ease: "none",
          scrollTrigger: {
            trigger: ".parallax-img",
            start: "top bottom",
            end: "bottom top",
            scrub: true,
            // markers:true
          },
        }
    );
});
</script>

<template>
  <div class="parallax-wrapper">
    <div ref="el">
      <PrismicImage :field="field" class="parallax-img"/>
    </div>
  </div>
</template>

<style scoped>
.parallax-wrapper {
  width: fit-content;
  height: fit-content;
  overflow: clip;
}

.parallax-img {
  display: block;
  width: 100%;
  height: 100%;
}

@media (max-width: 500px) {

  .parallax-img {
    width: 100%;
    height: 100%;
  }

}
</style>-->