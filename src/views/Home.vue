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
            answered: false,
            hit: false,
            start: 0,
            end: 14,
            question: 'Which social media is the best?',
            options: [
              {
                text: 'Facebook',
                frame: 1,
                correct: false
              },
              {
                text: 'Instagram',
                frame: 2,
                correct: false
              },
              {
                text: 'Twitter',
                frame: 3,
                correct: true
              }
            ]
          },
          {
            answered: false,
            hit: false,
            start: 15,
            end: 25,
            question: 'Which side you are?',
            options: [
              {
                text: 'PT',
                frame: 4,
                correct: false
              },
              {
                text: 'PSDB',
                frame: 5,
                correct: true
              }
            ]
          },
          {
            answered: false,
            hit: false,
            start: 26,
            end: 36,
            question: 'Bolsonaro is good.',
            options: [
              {
                text: 'True',
                frame: 5,
                correct: false
              },
              {
                text: 'False',
                frame: 6,
                correct: true
              }
            ]
          },
          {
            answered: false,
            hit: false,
            start: 37,
            end: 47,
            question: 'What to do when someone give you money?',
            options: [
              {
                text: 'Accept',
                frame: 6,
                correct: true
              },
              {
                text: 'Refuse',
                frame: 7,
                correct: true
              }
            ]
          },
          {
            answered: false,
            hit: false,
            start: 48,
            end: 58,
            question: 'What to do when someone give you drugs?',
            options: [
              {
                text: 'Accept',
                frame: 4,
                correct: false
              },
              {
                text: 'Refuse',
                frame: 6,
                correct: true
              }
            ]
          },
          {
            answered: false,
            hit: false,
            start: 59,
            end: 69
          },
          {
            answered: false,
            hit: false,
            start: 70,
            end: 80
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
