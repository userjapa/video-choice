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
        src: 'https://r2---sn-bg0ezn7s.googlevideo.com/videoplayback?requiressl=yes&id=c5489436f19fae6d&itag=18&source=blogger&app=blogger&ip=0.0.0.0&ipbits=0&expire=1543594741&sparams=expire,id,ip,ipbits,itag,mip,mm,mn,ms,mv,pl,requiressl,sc,source&signature=20BC23AC6DA4553C9512ECA5F9D340E973A846B0.2019E33A321203FAA8226865A697A3F4E8BFF8E6&key=cms1&cms_redirect=yes&mip=2001:1284:f01c:23e3:7d92:b5fd:6f94:ca1f&mm=30&mn=sn-bg0ezn7s&ms=nxu&mt=1541447137&mv=m&pl=33&sc=yes',
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
