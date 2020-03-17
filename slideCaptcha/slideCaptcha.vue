<template>
  <transition name="fade">
    <div
      :class="[
        'drag-verify-wrap',
        status === 'loading' && 'verify-scale',
        status === 'fail' && 'verify-bounce',
        status === 'error' && 'verify-scale',
        status === 'success' && 'verify-scale'
      ]"
      v-if="dialogVisible"
      v-loading="status === 'loading'"
    >
      <div class="drag-verify" v-if="showDragVerify">
        <div class="drag-verify-title">
          <span>拖动下方滑块完成拼图</span>
          <span>
            <i @click="reset" class="el-icon-refresh"></i>
            <i @click="close" class="el-icon-close"></i>
          </span>
        </div>
        <div class="drag-verify-body">
          <div class="shadeImage">
            <img v-if="this.shadeImage" :src="this.shadeImage" />
            <transition name="verify-tips">
              <p class="tips" v-show="status === 'fail'">请正确拼合图像</p>
            </transition>
          </div>
          <div
            :class="['cutoutImage', status === 'reset' && 'animate']"
            :style="{ top: this.imgY, left: this.sliderLeft + 'px' }"
            ref="cutoutImage"
            v-if="this.cutoutImage"
          >
            <img :src="this.cutoutImage" />
          </div>
          <div class="drag-verify-track">
            <div
              @mousedown="sliderDown"
              @touchstart="sliderDown"
              @touchmove="eventMove"
              @touchend="eventUp"
              ref="btn"
              :class="['drag-btn', status === 'reset' && 'animate']"
              :style="{ left: sliderLeft + 'px' }"
            >
              <el-button type="primary" round>| | |</el-button>
            </div>
          </div>
        </div>
      </div>
      <div class="drag-verify-mask" v-show="status === 'fail' || status === 'reset'"></div>
      <div class="drag-verify-mask-success" v-show="status === 'success'">
        <p><i class="el-icon-circle-check"></i>验证通过</p>
      </div>
      <div class="drag-verify-mask-fail" v-show="status === 'error'">
        <p>验证码已超时</p>
        <el-button type="primary" @click="reset"> 请点击重试</el-button>
      </div>
    </div>
  </transition>
</template>

<script>
import mockdata from './data.js'
export default {
  props: ['dialogVisible'],
  data() {
    return {
      originX: undefined,
      originY: undefined,
      isMouseDown: false,
      shadeImage: '',
      cutoutImage: '',
      imgX: 108,
      imgY: 0,
      imgW: 280,
      sliderLeft: 0,
      trail: [],

      status: null
    }
  },
  computed: {
    showDragVerify() {
      return !['success', 'loading', 'error'].includes(this.status)
    }
  },
  created() {
    this.bindEvents()
  },
  methods: {
    initImg() {
      this.status = 'loading'
      setTimeout(() => {
        this.shadeImage = 'data:image/jpg;base64,' + mockdata.shadeImage
        this.cutoutImage = 'data:image/jpg;base64,' + mockdata.cutoutImage
        this.imgY = mockdata.y + 'px'
        this.status = null
      }, 1000)
    },
    bindEvents() {
      document.addEventListener('mousemove', e => {
        this.eventMove(e)
      })
      document.addEventListener('mouseup', e => {
        this.eventUp(e)
      })
    },
    reset() {
      this.status = null
      this.trail = []
      this.sliderLeft = 0
    },

    close() {
      this.$emit('update:dialogVisible', false)
    },
    sliderDown(e) {
      if (e.type === 'touchstart') {
        this.originX = e.changedTouches[0].clientX
        this.originY = e.changedTouches[0].clientY
      } else {
        this.originX = e.clientX
        this.originY = e.clientY
      }
      this.isMouseDown = true
    },
    eventMove(e) {
      if (!this.isMouseDown) return false
      let moveX = 0
      let moveY = 0
      if (e.type === 'touchmove') {
        moveX = e.changedTouches[0].clientX - this.originX
        moveY = e.changedTouches[0].clientY - this.originY
      } else {
        moveX = e.clientX - this.originX
        moveY = e.clientY - this.originY
      }
      if (moveX < 0 || moveX + this.$refs.btn.offsetWidth >= this.imgW) return false
      this.sliderLeft = moveX
      this.trail.push(moveY)
    },
    eventUp(e) {
      if (!this.isMouseDown) return false
      this.isMouseDown = false
      const { spliced, TuringTest } = this.verify()
      if (spliced) {
        if (TuringTest) {
          this.status = 'success'
          setTimeout(() => {
            this.close()
            this.$emit('success')
          }, 600)
        } else {
          this.status = 'error'
        }
      } else {
        this.$emit('fail')
        this.status = 'fail'
        setTimeout(() => {
          this.status = 'reset'
          this.sliderLeft = 0
          setTimeout(() => {
            this.status = null
            this.trail = []
          }, 300)
        }, 1300)
      }
    },
    verify() {
      const arr = this.trail // drag y move distance
      const average = arr.reduce((x, y) => x + y) / arr.length // average
      const deviations = arr.map(x => x - average) // deviation array
      const stddev = Math.sqrt(deviations.map(x => x * x).reduce((x, y) => x + y) / arr.length) // standard deviation
      return {
        spliced: Math.abs(this.imgX - this.sliderLeft) < 4,
        TuringTest: average !== stddev // equal => not person operate
      }
    }
  },
  watch: {
    dialogVisible(val) {
      this.reset()
      if (val) {
        this.initImg()
      }
    }
  }
}
</script>
<style lang="scss">
.drag-verify-wrap {
  position: relative;
  width: 320px;
  height: 300px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  transform-origin: left bottom;
  transition: all 0.3s;
}
.drag-verify {
  width: 100%;
  height: 100%;
  &-mask {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: transparent;
    &-success,
    &-fail {
      text-align: center;
      line-height: 30px;
    }
    &-success {
      padding-top: 30px;
      color: #67c23a;
      font-size: 16px;
      p {
        display: flex;
        justify-content: center;
        align-items: center;
        i {
          font-size: 20px;
          margin-right: 4px;
        }
      }
    }
    &-fail p {
      margin-bottom: 30px;
    }
  }
  &-title {
    color: $color-main;
    font-size: 16px;
    line-height: 36px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .el-icon-refresh {
      cursor: pointer;
      margin-right: 15px;
    }
  }
  &-body {
    position: relative;
    .shadeImage {
      position: relative;
      width: 280px;
      height: 171px;
      overflow: hidden;
      .tips {
        padding-left: 15px;
        position: absolute;
        width: 100%;
        left: 0;
        bottom: 0;
        background: #f56c6c;
        color: #fff;
        z-index: 15;
      }
    }
    .cutoutImage {
      position: absolute;
      z-index: 10;
    }
    .shadeImage img,
    .cutoutImage img {
      width: 100%;
      height: 100%;
    }
  }
  &-track {
    position: relative;
    display: flex;
    align-items: center;
    width: 100%;
    height: 15px;
    background: rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    border-radius: 8px;
    margin-top: 25px;
    .drag-btn {
      position: absolute;
      .el-button {
        width: 56px;
        padding: 8px 16px;
      }
    }
  }
  .animate {
    transition: all 0.3s;
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: scale(0.7, 0.7);
}
.verify-tips-enter-active,
.verify-tips-leave-active {
  transition: all 0.3s;
}

.verify-tips-enter,
.verify-tips-leave-to {
  transform: translateY(100%);
}
.verify-bounce {
  animation: verifyBounce 0.3s cubic-bezier(0.095, 0.95, 0.82, 0.985);
}
.verify-scale {
  width: 200px;
  height: 150px;
}
@keyframes verifyBounce {
  0% {
    transform: translate(0, 0);
  }
  18% {
    transform: translate(-15px, 0);
  }
  36% {
    transform: translate(0px, 0);
  }
  54% {
    transform: translate(15px, 0);
  }
  72% {
    transform: translate(0, 0);
  }
  90% {
    transform: translate(-10px, 0);
  }
  100% {
    transform: translate(0, 0);
  }
}
</style>
