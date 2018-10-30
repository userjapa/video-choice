<template>
  <div class="video-choice">
    <div class="video-choice__player">
      <video ref="video"
             @playing="playing = true"
             @pause="playing = false"
             @timeupdate="time = $event.target.currentTime"/>
      <div class="video-choice__player__controls"
           :class="{ show: loading }">
        <div class="video-choice__player__controls__play">
          <div class="video-choice__player__controls__play__loader" v-if="loading">
            <span></span>
          </div>
          <div class="video-choice__player__controls__play__icon"
               :class="{
                 play: !playing,
                 pause: playing
               }"
               @click="playing ? $refs['video'].pause() : $refs['video'].play()"
               v-else>
          </div>
        </div>
        <div class="video-choice__player__controls__bottom" v-show="!loading">
          <div class="video-choice__player__controls__bottom__time">
            <input type="range"
                   step="0.0001"
                   v-model="time"
                   :min="start"
                   :max="duration">
            <div class="video-choice__player__controls__bottom__time__line">
              <div class="video-choice__player__controls__bottom__time__line__progress"
                   :style="{ width: `${(time * 100) / video.end}%` }">
                <div class="video-choice__player__controls__bottom__time__line__progress__ball">
                  <div class="video-choice__player__controls__bottom__time__line__progress__ball__small"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="video-choice__modal">
      <div class="video-choice__modal__content">

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VideoChoice',
  data () {
    return {
      time: 2,
      loading: false,
      playing: false,
      duration: 0,
      currentFrame: null,
      previousFrame: null
    }
  },
  props: {
    video: {
      type: Object,
      default: {}
    }
  },
  computed: {
    start () {
      if (!!previousFrame) {
        // this.
      }
    }
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
      transition: opacity .4s;
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
          height: 50%;
          width: 50%;
          margin: auto;
          background-size: cover;
          &.play {
            margin-left: 30px;
            background-image: url('../assets/play-button.svg');
          }
          &.pause {
            background-image: url('../assets/pause-button.svg');
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
              width: 10%;
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

@media only screen and (max-width: 600px) {
  .video-choice {
    &__player {
      &__controls {
        &__play {
          top: calc(50% - 25px);
          left: calc(50% - 25px);
          width: 50px;
          height: 50px;
        }
        &__bottom {
          height: 30px;
        }
      }
    }
  }
}
</style>
