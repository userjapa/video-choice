<template>
  <div class="create">
    <div class="create__preview">
      <VideoChoice ref="game" :video="video"/>
    </div>
    <form class="create__form" @submit.prevent="" v-if="!!game">
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
          <label for="frame-start">Start</label>
        </div>
        <div class="create__form__field__input">
          <input id="frame-start" type="number" v-model="frame.start" step="1" min="0">
        </div>
      </div>
      <div class="create__form__field">
        <div class="create__form__field__label">
          <label for="frame-end">End</label>
        </div>
        <div class="create__form__field__input">
          <input id="frame-end" type="number" v-model="frame.end" step="1" :min="parseFloat(frame.start) + 1">
        </div>
      </div>
      <div class="create__form__field">
        <div class="create__form__field__btn">
          <button type="button" @click="addFrame(frame)" :disabled="!isFrameValid">Add Frame</button>
        </div>
      </div>
      <div class="create__form__field">
        <div class="create__form__field__items">
          <div class="create__form__field__items__frame" v-for="f in video.frames">
            {{ f.start }} - {{ f.end }}
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
        answered: false,
        hit: false,
        start: 0,
        end: 0
      },
      game: null
    }
  },
  computed: {
    duration () {
      return !this.game ? 0 : this.game.fullDuration
    },
    isFrameValid () {
      let isValid = true
      if (!this.video.src || !this.duration) isValid = false
      if (this.frame.start >= this.frame.end) isValid = false
      if (!!this.video.frames && !!this.video.frames.length) {
        if (this.video.frames[this.video.frames.length - 1].end <= this.frame.start) isValid = false
      }
      return isValid
    }
  },
  methods: {
    addFrame (frame) {
      this.video.frames.push(cloneDeep(frame))
      this.frame = { start: 0, end: 0 }
    }
  },
  components: {
    VideoChoice
  },
  watch: {
    duration (duration) {
      this.frame.end = duration.toFixed(0)
    },

  },
  mounted () {
    this.game = this.$refs['game']
  }
}
</script>

<style lang="scss">
.create {
  display: grid;
  grid-template-columns: 1fr 1fr;
  &__preview {
    padding: 10px 5px 10px 10px;
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
    }
  }
}
</style>
