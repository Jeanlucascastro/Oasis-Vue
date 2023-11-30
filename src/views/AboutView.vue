'use strict'
<template>
  <div class="about">
    <h1>This is an about page {{ loop }} {{ userName }}</h1>
    <YoutubeVue3 ref="youtube" :videoid="video_id" :loop="loop" :width="480" :height="320"
     @ended="onEnded" @paused="onPaused" @played="onPlayed"/>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

<script lang="ts">
import { YoutubeVue3 } from 'youtube-vue3'
import { useRoute } from 'vue-router';
import { ref } from 'vue';
import { useLoginMixin, type LoginMixin } from '../mixins/LoginMixin.js';


const { checkLogin }: LoginMixin = useLoginMixin();

export default {

 name: 'App',
 data() {
   return {
     video_id: "kwpGBT-cQ-M",
     userName: localStorage.getItem('usuario-oasis') 
   }
 },
 setup() {
  const route = useRoute();
  const loop = ref(1);

  
  if (Array.isArray(route.params.id)) {
    // handle array case
  } else {
    loop.value = Number(route.params.id);
  }
  
  return { loop };
 },
 components: {
   YoutubeVue3,
 },
 methods: {
   onEnded() {
     console.log("## OnEnded")
   },
   onPaused() {
     console.log("## OnPaused")
   },
   onPlayed() {
     console.log("## OnPlayed")
   },
 },

 created() {
    checkLogin()
  },
}

</script>
