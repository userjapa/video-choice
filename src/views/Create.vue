<template>
  <div class="create">
    <div class="create__preview">
      <div class="create__preview__video">
        <video ref="video"
               controls
               :src="video.src"
               @loadeddata="duration = $event.target.duration"
               @timeupdate="timeUpdated($event.target.currentTime)"/>
        <transition name="question_transition">
          <div class="create__preview__video__question" v-show="showQuestion">
            <div class="create__preview__video__question__text">
              <!-- <input type="text" v-model="question.text"> -->
            </div>
            <div class="create__preview__video__question__answers">
              <!-- <div class="create__preview__video__question__answers__item" v-for="aswr in question.answers">
                {{ aswr.text }}
              </div> -->
            </div>
          </div>
        </transition>
      </div>
    </div>
    <form class="create__form" @submit.prevent="">
      <span>Video</span>
      <div class="create__form__field">
        <div class="create__form__field__label">
          <label for="video-source">Video Source</label>
        </div>
        <div class="create__form__field__input">
          <input id="video-source" type="text" v-model="video.src">
        </div>
      </div>
      <hr>
      <span>Frame</span>
      <div class="create__form__field">
        <div class="create__form__field__label">
          <label for="frame-end">End</label>
        </div>
        <div class="create__form__field__input">
          <input id="frame-end" type="number" v-model="frame.end" step="1" :min="parseFloat(frame.start) + 1" :max="duration">
          <button type="button" @click="getCurrentTime()">Get Current Time</button>
        </div>
      </div>
      <div class="create__form__field">
        <div class="create__form__field__btn">
          <button type="button" @click="addFrame(frame)" :disabled="!isFrameValid">Add Frame</button>
        </div>
      </div>
      <div class="create__form__field">
        <div class="create__form__field__items">
          <div class="create__form__field__items__frame" v-for="f in video.frames" @click="setFrame(f)">
            <div class="create__form__field__items__frame__rate">
              <p>{{ f.start }} - {{ f.end }}</p>
            </div>
            <div class="create__form__field__items__frame__question">
              <div class="create__form__field__items__frame__question__text">
                <input type="text" name="" value="">
              </div>
              <div class="create__form__field__items__frame__question__answers">

              </div>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { cloneDeep } from 'lodash'
import VideoChoice from './../components/VideoChoice'

export default {
  name: 'create',
  data () {
    return {
      video: {
        src: '',
        frames: []
      },
      frame: {
        start: 0,
        end: 0,
        question: {
          text: '',
          answers: []
        }
      },
      question: {
        text: '',
        answers: []
      },
      answer: {
        correct: false,
        text: '',
        frame: 0
      },
      currentFrame: null,
      min: 0,
      duration: 0,
      time: 0,
      showQuestion: false
    }
  },
  computed: {
    isFrameValid () {
      let isValid = true
      if (!this.video.src || !this.duration) isValid = false
      if (!!this.video.frames && !!this.video.frames.length) {
        if (this.video.frames[this.video.frames.length - 1].start >= this.duration) isValid = false
      }
      return isValid
    }
  },
  methods: {
    addFrame (frame) {
      if (frame.start < this.duration) {
        this.video.frames.push(cloneDeep(frame))
        this.frame = { start: 0, end: 0 }
      }
    },
    setFrame (frame) {
      this.currentFrame = frame
      this.$refs['video'].currentTime = !!frame ? frame.start : 0
    },
    getCurrentTime () {
      this.frame.end = parseFloat(this.$refs['video'].currentTime.toFixed(0))
    },
    timeUpdated (time) {
      if (this.showQuestion) this.showQuestion = false
      this.time = parseFloat(time.toFixed(0))
      if (!!this.currentFrame) {
        if (this.time == parseFloat(this.currentFrame.end)) {
          this.$refs['video'].pause()
          this.showQuestion = true
        }
      }
    }
  },
  components: {
    VideoChoice
  },
  watch: {
    duration (duration) {
      this.frame.end = parseFloat(duration.toFixed(0))
    },
    'video.frames' (frames) {
      if (!frames.length) this.min = 0
      else this.min = parseFloat(frames[frames.length - 1].end) + 1
      this.frame.start = this.min
      this.frame.end = parseFloat(this.duration.toFixed(0))
    }
  },
}
</script>

<style lang="scss">
.create {
  display: grid;
  grid-template-columns: 1.75fr 1fr;
  &__preview {
    padding: 10px 5px 10px 10px;
    video {
      min-width: 100%;
      max-width: 100%;
      max-height: 100%;
      background-color: rgba(#000, .5);
    }
    &__video {
      position: relative;
      &__question {
        position: absolute;
        display: flex;
        flex-direction: column;
        top: 0;
        left: 0;
        background-color: #ddd;
        margin: 50px 100px 0px 100px;
        border-radius: 20px;
        box-shadow: 0px 0px 20px 0px #000;
        width: calc(100% - 200px);
        height: calc(100% - 150px);
        &__text {
          border: 2px solid red;
          flex-basis: 30%;
          display: flex;
          justify-content: center;
          align-items: center;
          input {
            text-align: center;
            padding: 5px 15px;
            font-size: 1.6rem;
            background-color: rgba(#eee, .75);
            outline: none;
            border-radius: 5px;
            width: calc(100% - 100px);
          }
        }
        &__answers {
          border: 2px solid red;
          flex-basis: 70%;
          &__item {
            border: 2px solid red;
          }
        }
      }
    }
  }
  &__form {
    padding: 10px 10px 10px 5px;
    &__field {
      display: flex;
      justify-content: center;
      align-items: center;
      border: 2px solid black;
      padding: 5px;
      &__label {
        flex-basis: calc(42.5%);
        text-align: right;
        label {
          margin-right: 5px;
        }
      }
      &__input {
        flex-basis: calc(57.5%);
        input {
          width: 200px;
          float: left;
          padding: 5px 10px;
          border: 2px solid #ddd;
          outline: none;
        }
      }
      &__btn {
        flex-grow: 1;
        button {
          cursor: pointer;
          padding: 7.5px 25px;
          border: 2px solid #fff;
          border-radius: 5px;
          outline: none;
          transition: all .2s;
          &:hover {
            background-color: #fff;
            border-color: #eee;
          }
          &:active {
            background-color: #eee;
          }
          &:disabled {
            cursor: not-allowed;
            background-color: #eee;
            color: #aaa;
          }
        }
      }
      &__items {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        &__frame {
          display: flex;
          justify-content: center;
          align-items: center;
          align-self: center;
          width: 400px;
          height: 50px;
          border: 2px solid red;
          &:not(:last-child) {
            margin-bottom: 5px;
          }
          &__rate {
            border: 2px solid red;
            flex-basis: 25%;
          }
          &__question {
            flex-basis: 75%;
            &__text {

            }
            &__answers {

            }
          }
        }
      }
    }
    &__question {
      display: flex;
      flex-direction: column;
    }
  }
}

.question_transition {
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
</style>
