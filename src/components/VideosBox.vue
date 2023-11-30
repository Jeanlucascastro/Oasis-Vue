<template>
<div class="global-box">

  <button @click="getVideos(2)">Baixar</button>


  <div v-for="video of videos">
    {{ video.name }}
  </div>

</div>


</template>

<script lang="ts">
import axios from 'axios'

export default {

name: 'VideoBox',
data() {
  return {
    video_id: "kwpGBT-cQ-M",
    videos: [] as any
  }
},
props: {
    courseId: Number
},
methods: {
  async getVideos(id: number) {
    const token = localStorage.getItem('token-oasis')
      try {
        await axios
          .get(
            'http://192.168.0.104:8080/video/byCourse/' + id,
            {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': 'Origin, X-Requested-With, Content-Type, Accept',
                'Authorization': `Bearer ${token}`
              }
            }
          )
          .then((videos) => {
            this.videos = videos.data;
            console.log('videos.data', videos.data)
            console.log('this.videos', videos.data)
          })
      } catch (error) {
        console.error('Error fetching data:', error)
      }
    },
}
}
</script>