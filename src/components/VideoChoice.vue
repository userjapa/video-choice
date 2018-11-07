<template>
  <div class="video-choice">
    <div class="video-choice__player">
      <video ref="video"
             @loadeddata="loading = false; fullDuration = $event.target.duration"
             @loadstart="loading = true"
             @playing="playing = true"
             @pause="playing = false"
             @timeupdate="timeUpdated($event.target)" muted/>
      <div class="video-choice__player__controls"
           :class="{ show: (loading || showOptions || !playing) }">
        <transition name="fade">
        <div class="video-choice__player__controls__play" key="play" v-if="!showOptions">
          <div class="video-choice__player__controls__play__loader" v-if="loading">
            <span></span>
          </div>
          <div class="video-choice__player__controls__play__icon"
               :class="{
                 play: !playing,
                 pause: playing
               }"
               @click="play()"
               v-else>
          </div>
        </div>
        <div class="video-choice__player__controls__options" key="options" v-else>
          <div class="video-choice__player__controls__options__question">
            {{ currentFrame.question }}
          </div>
          <div class="video-choice__player__controls__options__items">
            <div class="video-choice__player__controls__options__items__answer"
                 v-for="(op, index) in currentFrame.options"
                 :style="{ flexBasis: `${100 / currentFrame.options.length}%` }"
                 @click="select(op)">
              {{ op.text }}
            </div>
          </div>
          <transition name="fade">
          <div class="video-choice__player__controls__options__feedback"
               :class="{
                 true: optionIsCorrect,
                 false: !optionIsCorrect,
                 show: showFeedback
               }"
               v-show="showFeedback">
          </div>
          </transition>
        </div>
        </transition>
        <div class="video-choice__player__controls__bottom" v-show="!loading">
          <div class="video-choice__player__controls__bottom__time">
            <div ref="line"
                 class="video-choice__player__controls__bottom__time__line"
                 @click="changeRate">
              <div class="video-choice__player__controls__bottom__time__line__progress"
                   :style="{ width: `${percent}%` }">
                <div ref="ball"
                     class="video-choice__player__controls__bottom__time__line__progress__ball"
                     @mousedown.prevent="dragRate">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'VideoChoice',
  data () {
    return {
      time_old: 0,
      time: 0,
      volume: 0,
      loading: false,
      playing: false,
      currentFrame: null,
      previousFrame: null,
      showOptions: false,
      ended: false,
      optionIsCorrect: false,
      showFeedback: false,
      fullDuration: 0
    }
  },
  props: {
    video: {
      type: Object,
      default: () => ({})
    }
  },
  computed: {
    duration () {
      return !this.currentFrame ? this.fullDuration : this.currentFrame.end - this.currentFrame.start
    },
    percent () {
      let percent = 0
      if (this.currentFrame) percent = this.duration ? (((this.time - this.currentFrame.start) * 100) / this.duration) : 0
      else percent = (this.time * 100) / this.fullDuration
      if (isNaN(percent)) percent = 0
      return percent > 100 ? 100 : percent
    }
  },
  methods: {
    play () {
      if (this.playing) this.$refs['video'].pause()
      else {
        if (this.ended && (this.currentFrame.end >= this.time_old.toFixed(0)) && (this.currentFrame.end <= this.time.toFixed(0))) {
          this.$refs['video'].currentTime = this.currentFrame.start
        }
        this.$refs['video'].play()
      }
    },
    async setFrame (index) {
      if (!!this.video.frames && this.video.frames.length) {
        this.$refs['video'].currentTime = this.video.frames[index].start
        if (this.showOptions) this.showOptions = false
        if (!this.currentFrame) {
          this.$set(this, 'currentFrame', this.video.frames[index])
        } else {
          this.$set(this, 'previousFrame', this.currentFrame)
          this.$set(this, 'currentFrame', this.video.frames[index])
          this.play()
        }
        if (!this.currentFrame.options || !this.currentFrame.options.length) this.ended = true
        else this.currentFrame.options.map(op => (this.$set(op, 'selected', false)))
      }
    },
    timeUpdated (video) {
      if (this.currentFrame) {
        this.$set(this, 'time_old', this.time)
        this.$set(this, 'time', video.currentTime)
        if (this.currentFrame.end >= this.time_old && this.currentFrame.end <= this.time) {
          video.pause()
          if (!!this.currentFrame.options && !!this.currentFrame.options.length) this.showOptions = true
        }
      } else {
        this.$set(this, 'time_old', this.time)
        this.$set(this, 'time', video.currentTime)
      }
    },
    changeRate (ev) {
      if (!ev.target.classList.contains('video-choice__player__controls__bottom__time__line__progress__ball')) {
        const el = this.$refs['line']
        const width = el.scrollWidth - 1
        const position = ev.pageX - 25
        if (this.currentFrame) this.$refs['video'].currentTime = ((this.duration * position) / width) + this.currentFrame.start
        else this.$refs['video'].currentTime = ((this.fullDuration * position) / width)
        if (this.showOptions) {
          setTimeout(() => {
            this.showOptions = false
            this.play()
          }, 500)
        }
      }
    },
    dragRate (ev) {
      if (this.showOptions) this.showOptions = false
      const wasPlaying = this.playing
      const video = this.$refs['video']
      const line = this.$refs['line']
      const ball = this.$refs['ball']
      const positionX = ev.clientX - ball.getBoundingClientRect().left

      video.pause()

      const onMouseMove = (ev) => {
        let position = ev.clientX - positionX - line.getBoundingClientRect().left
        const rightEdge = line.offsetWidth

        if (position < 0) position = 0
        if (position > rightEdge) position = rightEdge
        if (this.currentFrame) video.currentTime = ((this.duration * position) / line.scrollWidth) + this.currentFrame.start
        else video.currentTime = ((this.duration * position) / line.scrollWidth)
      }

      const onMouseUp = () => {
        if (wasPlaying) video.play()
        document.removeEventListener('mouseup', onMouseUp)
        document.removeEventListener('mousemove', onMouseMove)
      }

      document.addEventListener('mousemove', onMouseMove)
      document.addEventListener('mouseup', onMouseUp)
    },
    select (option) {
      if (!option.selected) {
        this.$set(option, 'selected', true)
        this.optionIsCorrect = option.correct
        this.showFeedback = true
        setTimeout(() => {
          this.showFeedback = false
          setTimeout(() => (this.setFrame(option.frame)), 500)
        }, 2000)
      }
    },
    initVideo () {
      this.setFrame(0)
      this.$refs['video'].src = this.video.src
      this.$refs['video'].load()
    }
  },
  watch: {
    'video.src' () {
      this.initVideo()
    },
    'video.frames' (frames) {
      if (!!frames && !!frames.length) this.setFrame(frames.length - 1)
    }
  },
  mounted () {
    this.initVideo()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.video-choice {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 100%;
  max-height: 100%;
  &__player {
    position: relative;
    flex-grow: 1;
    align-self: stretch;
    display: flex;
    justify-content: center;
    align-items: center;
    video {
      position: relative;
      min-width: 100%;
      max-width: 100%;
      min-height: 100%;
      max-height: 100%;
      background-color: rgba(#ddd, .5);
    }
    &__controls {
      position: absolute;
      display: flex;
      top: 0;
      left: 0;
      justify-content: center;
      align-items: center;
      background-color: rgba(#000, .5);
      height: 100%;
      width: 100%;
      opacity: 0;
      transition: opacity .6s;
      &.show {
        opacity: 1;
      }
      &:hover {
        opacity: 1;
      }
      &__play {
        display: flex;
        justify-content: center;
        align-items: center;
        position: absolute;
        background-color: #fff;
        top: calc(50% - 50px);
        left: calc(50% - 50px);
        width: 100px;
        height: 100px;
        border-radius: 150px;
        &__loader {
          height: 36px;
        	width: 36px;
        	animation: loader-1 2s cubic-bezier(0.770, 0.000, 0.175, 1.000) infinite;
          &:before {
          	content: "";
          	display: block;
          	position: absolute;
          	top: 0; left: 0;
          	bottom: 0; right: auto;
          	margin: auto;
          	width: 24px;
          	height: 24px;
          	background: #888;
          	border-radius: 50%;
            animation: loader-2 2s cubic-bezier(0.770, 0.000, 0.175, 1.000) infinite;
          }
          &:after {
          	content: "";
          	display: block;
          	position: absolute;
          	top: 0; left: auto;
          	bottom: 0; right: 0;
          	margin: auto;
          	width: 24px;
          	height: 24px;
          	background: #888;
          	border-radius: 50%;
            animation: loader-3 2s cubic-bezier(0.770, 0.000, 0.175, 1.000) infinite;
          }
          span {
          	display: block;
          	position: absolute;
          	top: 0; left: 0;
          	bottom: 0; right: 0;
          	margin: auto;
          	height: 32px;
          	width: 32px;
            &:before {
            	content: "";
            	display: block;
            	position: absolute;
            	top: 0; left: 0;
            	bottom: auto; right: 0;
            	margin: auto;
            	width: 24px;
            	height: 24px;
            	background: #888;
            	border-radius: 50%;
              animation: loader-4 2s cubic-bezier(0.770, 0.000, 0.175, 1.000) infinite;
            }
            &:after {
              content: "";
              display: block;
              position: absolute;
              top: auto; left: 0;
              bottom: 0; right: 0;
              margin: auto;
              width: 24px;
              height: 24px;
              background: #888;
              border-radius: 50%;
              animation: loader-5 2s cubic-bezier(0.770, 0.000, 0.175, 1.000) infinite;
            }
          }
        }
        &__icon {
          cursor: pointer;
          height: 50%;
          width: 50%;
          margin: auto;
          background-size: cover;
          &.play {
            margin-left: 30%;
            background-image: url('../assets/play-button.svg');
          }
          &.pause {
            background-image: url('../assets/pause-button.svg');
          }
        }
      }
      &__options {
        position: relative;
        display: grid;
        grid-template-rows: 1fr 3fr;
        height: calc(100% - #{250px});
        width: calc(100% - #{500px});
        background-color: #eee;
        border-radius: 15px;
        &__question {
          background-color: #ccc;
          margin: 30px 60px;
          border-radius: 15px;
          color: #fff;
          font-size: 2rem;
          display: flex;
          justify-content: center;
          align-items: center;
        }
        &__items {
          display: flex;
          justify-content: space-between;
          background-image: linear-gradient(to top, #ddd 70%, #ccc);
          margin: 0px 60px 30px 60px;
          border-radius: 15px;
          padding: 30px;
          &__answer {
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 30px 0;
            border-radius: 15px;
            background-color: rgba(#fff, .75);
            color: #999;
            font-size: 1.6rem;
            transition: all .2s;
            box-shadow: 0px 0px 5px 0px #888;
            &:not(:last-child) {
              margin-right: 30px;
            }
            &:hover {
              box-shadow: 0px 0px 25px 0px #888;
            }
          }
        }
        &__feedback {
          position: absolute;
          display: flex;
          justify-content: center;
          align-items: center;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          background-color: #fff;
          border-radius: 15px;
          font-size: 2.4rem;
          opacity: .85;
          &.true:after {
              content: 'True';
              color: #5EC12D;
          }
          &.false:after {
              content: 'False';
              color: #C2003C;
          }
        }
      }
      &__bottom {
        position: absolute;
        display: flex;
        bottom: 0;
        width: 100%;
        height: 50px;
        &__time {
          position: relative;
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 0 25px;
          width: calc(100% - #{50px});
          background-image: linear-gradient(180deg, rgba(#000, 0), rgba(#000, .1), rgba(#000, .5));
          input {
            display: none;
          }
          &__line {
            background-color: #ddd;
            border-radius: 15px;
            height: 3px;
            width: 100%;
            transition: all .2s;
            cursor: pointer;
            &__progress {
              position: relative;
              height: 100%;
              background-color: #888;
              border-radius: 15px;
              &__ball {
                cursor: pointer;
                position: absolute;
                top: -3px;
                right: -3px;
                background-color: #bbb;
                width: 10px;
                height: 10px;
                border-radius: 25px;
                transition: all .2s;
              }
            }
          }
        }
        &:hover {
          .video-choice__player__controls__bottom__time__line {
            height: 6px;
            &__progress__ball {
              top: -5px;
              right: -5px;
              height: 16px;
              width: 16px;
            }
          }
        }
      }
    }
  }
}

.fade {
  &-enter-active {
    animation: fadeOut .4s ease backwards;
    animation-delay: .4s;
  }
  &-leave-active {
    animation: fadeIn .4s ease backwards;
  }
}

@keyframes fadeOut {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeIn {
  from { opacity: 1; }
  to { opacity: 0; }
}

@keyframes loader-1 {
	0%   { transform: rotate(0deg); }
	100% { transform: rotate(360deg); }
}

@keyframes loader-2 {
	0%   { transform: translate3d(-15px, 0, 0) scale(.5); }
	50%  { transform: translate3d(36px, 0, 0) scale(1); }
	100% { transform: translate3d(-15px, 0, 0) scale(.5); }
}

@keyframes loader-3 {
	0%   { transform: translate3d(15px, 0, 0) scale(.5); }
	50%  { transform: translate3d(-36px, 0, 0) scale(1); }
	100% { transform: translate3d(15px, 0, 0) scale(.5); }
}

@keyframes loader-4 {
	0%   { transform: translate3d(0, -15px, 0) scale(.5); }
	50%  { transform: translate3d(0, 36px, 0) scale(1); }
	100% { transform: translate3d(0, -15px, 0) scale(.5); }
}

@keyframes loader-5 {
	0%   { transform: translate3d(0, 15px, 0) scale(.5); }
	50%  { transform: translate3d(0, -36px, 0) scale(1); }
	100% { transform: translate3d(0, 15px, 0) scale(.5); }
}

@media only screen and (max-width: 768px) {
  .video-choice {
    &__player {
      &__controls {
        &__play {
          top: calc(50% - 40px);
          left: calc(50% - 40px);
          width: 80px;
          height: 80px;
        }
        &__bottom {
          height: 30px;
        }
        &__options {
          margin-bottom: 30px;
          border-radius: 0px 0px 5px 5px;
          width: 100%;
          height: calc(100% - #{30px});
          &__question {
            font-size: 1.4rem;
            padding: 5px;
          }
        }
      }
    }
  }
}
</style>
