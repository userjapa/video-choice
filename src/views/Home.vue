<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <VideoChoice :video="video"/>
  </div>
</template>

<script>
import VideoChoice from '@/components/VideoChoice.vue'

export default {
  name: 'home',
  data () {
    return {
      video: {
        src: 'https://www.blogger.com/video-play.mp4?contentId=c294ea21682074f1',
        frames: [
          {
            start: 4,
            end: 14,
            options: [
              {
                text: 'Facebook',
                frame: 1
              },
              {
                text: 'Instagram',
                frame: 2
              },
              {
                text: 'Twitter',
                frame: 3
              }
            ]
          },
          {
            start: 24,
            end: 34,
            options: [
              {
                text: 'PT',
                frame: 4
              },
              {
                text: 'PSDB',
                frame: 5
              }
            ]
          },
          {
            start: 44,
            end: 54,
            options: [
              {
                text: 'True',
                frame: 5
              },
              {
                text: 'False',
                frame: 6
              }
            ]
          },
          {
            start: 64,
            end: 74,
            options: [
              {
                text: 'Accept',
                frame: 6
              },
              {
                text: 'Refuse',
                frame: 7
              }
            ]
          },
          {
            start: 84,
            end: 94,
            options: [
              {
                text: 'Accept',
                frame: 4
              },
              {
                text: 'Refuse',
                frame: 6
              }
            ]
          },
          {
            start: 104,
            end: 114
          },
          {
            start: 124,
            end: 134
          }
        ]
      }
    }
  },
  components: {
    VideoChoice
  }
}
</script>
