<script setup lang="ts">
import { asImageSrc } from "@prismicio/client";
import { components } from "~/slices";

import NavBar from "~/components/NavBar.vue";

const { client } = usePrismic();
const { data: page } = await useAsyncData("[project_gallery]", () =>
    client.getSingle("project_gallery"),
);

useSeoMeta({
  title: page.value?.data.meta_title,
  ogTitle: page.value?.data.meta_title,
  description: page.value?.data.meta_description,
  ogDescription: page.value?.data.meta_description,
  ogImage: computed(() => asImageSrc(page.value?.data.meta_image)),
});
</script>

<template data-theme="dark">
  <div class="page">
    <nav-bar/>
    <h1 id="title"><PrismicRichText :field="page?.data.title"/></h1>
    <div class="scrolling">
      <SliceZone
          id="slicesZone"
          wrapper="main"
          :slices="page?.data.slices ?? []"
          :components="components"
      />
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  background-color: var(--color-primary);
  overflow-x: hidden;
}
.page {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  align-content: center;
}
.scrolling{
  position:relative;
  flex:1;
  overflow:auto;
  width:100%;
}

#title p,
#title h1,
#title h2,
h1{
  margin: 0;
}
#title{
  position: relative;
  color: var(--color-secondary);
  font-family: 'TheSeasons', serif;
  font-size: 96px;
  font-weight: normal;
  justify-self: center;
  margin: 6% 0 2% 0;
}

@media (max-width:768px){

  #title{
    font-size:52px;
    text-align:center;
    line-height:1.1;
    margin:90px 0 32px;
    padding:0 20px;
  }

  .scrolling{
    overflow:visible;
  }
}
</style>