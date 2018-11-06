<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <div class="home__video">
      <VideoChoice :video="video"/>
    </div>
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
                correct: false,
                selected: false
              },
              {
                text: 'Instagram',
                frame: 2,
                correct: false,
                selected: false
              },
              {
                text: 'Twitter',
                frame: 3,
                correct: true,
                selected: false
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
                frame: 0,
                correct: false,
                selected: false
              },
              {
                text: 'PSDB',
                frame: 5,
                correct: true,
                selected: false
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
                correct: false,
                selected: false
              },
              {
                text: 'False',
                frame: 6,
                correct: true,
                selected: false
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
                frame: 4,
                correct: true,
                selected: false
              },
              {
                text: 'Refuse',
                frame: 6,
                correct: true,
                selected: false
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
                frame: 2,
                correct: false,
                selected: false
              },
              {
                text: 'Refuse',
                frame: 5,
                correct: true,
                selected: false
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

<style lang="scss">
.home {
  &__video {
    display: flex;
    justify-content: center;
    align-items: center;
    height: calc(100% - #{278px});
  }
}
</style>
