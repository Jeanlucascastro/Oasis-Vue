'use strict'
<template>
  <div class="about">
    <h1>Aula  {{ loop }} {{ userName }}</h1>
    <YoutubeVue3 ref="youtube" :videoid="video.url" :loop="0" :width="480" :height="320"/>
  </div>
</template>

<style>
/* @media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
} */
</style>

<script lang="ts">
import { YoutubeVue3 } from 'youtube-vue3'
import { useRoute } from 'vue-router';
import { ref  } from 'vue';
import { useLoginMixin, type LoginMixin } from '../mixins/LoginMixin.js';
import axios from 'axios'



const { checkLogin }: LoginMixin = useLoginMixin();

export default {

 name: 'App',
 data() {
   return {
    video: {} as any,
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
  async getVideos(id: any) {
    const token = localStorage.getItem('token-oasis')
      try {
        await axios
          .get(
            'http://192.168.0.104:8080/video/' + id,
            {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Origin, X-Requested-With, Content-Type, Accept',
                'Authorization': `Bearer ${token}`
              }
            }
          )
          .then((video) => {
            this.video = video.data;
            console.log('videos.data', video.data)
            console.log('this.videos', video.data)
          })
      } catch (error) {
        console.error('Error fetching data:', error)
      }
    },
 },

 created() {
    checkLogin()
    this.getVideos(this.loop)
  },
}

</script>
