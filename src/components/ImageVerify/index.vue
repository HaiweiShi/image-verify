<template>
  <div class="image-verify">
    <div
      class="model"
      :style="{
        zIndex,
        position: float ? 'fixed': ''
      }"
      v-show="!float || modelFlag"
    >
      <div v-if="float" class="shade" :style="{zIndex: 1}" />
      <div class="content">
        <div
          @mousedown.prevent="areaMousedown" @mousemove.prevent="areaMousemove" @click.prevent="dragEnd"
          @touchstart.prevent='areaTouchstart' @touchmove.prevent="areaTouchmove" @touchend.prevent="dragEnd"
        >
          <div class="entire-image" :style="style1" />
          <div @mousedown.stop @touchstart.stop :style="style3" class="valid-area" />
          <div :style="style2" class="valid-block" />
        </div>
        <div v-show="messageFlag" class="message-div">{{message}}</div>
        <div class="btn">
          <div @click="resetValidArea">重置</div>
          <div v-if="float" @click="closeModel">取消</div>
        </div>
      </div>
    </div>
    <div @click="showModel">
      <slot />
    </div>
  </div>
</template>
<script>
import img1 from '@/assets/img1.jpg'
import img2 from '@/assets/img2.jpg'
import img3 from '@/assets/img3.jpg'
// import img4 from '@/assets/img4.png'
// import img5 from '@/assets/img5.png'
// 清空浏览器选中状态
var clearSlct =
  'getSelection' in window
    ? function () {
      window.getSelection().removeAllRanges()
    }
    : function () {
      document.selection.empty()
    }

export default {
  name: 'image-verify',
  props: {
    value: {
      type: Boolean,
      default: false
    },
    imageList: {
      type: Array,
      default: () => [img1, img2, img3]
    },
    zIndex: {
      type: Number,
      default: 2019
    },
    float: {
      type: Boolean,
      default: true
    },
    timeout: {
      type: Number,
      default: 0
    },
    width: {
      type: Number,
      default: 368
    },
    height: {
      type: Number,
      default: 200
    },
    areaSize: {
      type: Number,
      default: 50
    }
  },
  data () {
    return {
      imgSrc: null,
      modelFlag: false,
      messageFlag: false,
      message: '验证成功',
      areaX: 0,
      areaY: 0,
      blockX: 0,
      blockY: 0,
      tempX: 0,
      tempY: 0,
      clickFalg: false,
      clickTime: null
    }
  },
  methods: {
    showModel () {
      this.modelFlag = true
      this.resetValidArea()
    },
    closeModel () {
      this.modelFlag = false
    },
    func () {
      if (this.clickFalg) this.dragEnd()
    },
    alertMessage (msg, func) {
      if (!msg) return
      this.messageFlag = true
      this.message = msg
      setTimeout(() => {
        this.messageFlag = false
        func && func()
      }, 1000)
    },
    setValue (v, time) {
      this.$emit('input', v)
      if (time) this.$emit('change', v, time)
    },
    areaTouchstart (e) {
      if (!this.value) {
        var touch = e.targetTouches[0]
        this.dragStart(touch)
      }
    },
    areaMousedown (e) {
      if (!this.value) {
        this.dragStart(e)
      }
    },
    areaTouchmove (e) {
      if (this.clickFalg) {
        var touch = e.targetTouches[0]
        var blockX = touch.clientX - this.tempX
        var blockY = touch.clientY - this.tempY
        this.dragMove(blockX, blockY)
      }
    },
    areaMousemove (e) {
      if (this.clickFalg) {
        let blockX = (e.clientX - this.tempX)
        let blockY = (e.clientY - this.tempY)
        this.dragMove(blockX, blockY)
      }
    },
    dragStart (e) {
      this.tempX = e.clientX // - e.currentTarget.offsetLeft;
      this.tempY = e.clientY // - e.currentTarget.offsetTop;
      this.clickFalg = true
      this.clickTime = Date.now()
    },
    dragMove (blockX, blockY) {
      let width = this.AWidth - this.aSize
      let height = this.AHeight - this.aSize
      this.blockX = blockX < 0 ? 0 : blockX > width ? width : blockX
      this.blockY = blockY < 0 ? 0 : blockY > height ? height : blockY
    },
    dragEnd (e) {
      if (this.clickFalg) {
        let DValue1 = Math.abs(this.blockX - this.areaX)
        let DValue2 = Math.abs(this.blockY - this.areaY)
        let time = Date.now() - this.clickTime
        let value = DValue1 < 8 && DValue2 < 8
        if (this.timeout && (this.timeout < time)) {
          value = false
          this.alertMessage(`验证超时，请在${(this.timeout / 1000).toFixed(2)}秒内完成校验`)
        }
        this.setValue(value, time)
        if (value) {
          this.alertMessage(
            `验证成功，用时${(time / 1000).toFixed(2)}秒`,
            this.closeModel
          )
          this.blockX = this.areaX
          this.blockY = this.areaY
        } else {
          this.blockX = 2
          this.blockY = 2
        }
        this.clickTime = null
        this.clickFalg = false
      }
      clearSlct()
    },
    resetValidArea () {
      this.imgSrc = this.imageList[parseInt(Math.random() * this.imageList.length)]
      this.areaX = parseInt(Math.random() * (this.AWidth - this.aSize - 100) + 50)
      this.areaY = parseInt(Math.random() * (this.AHeight - this.aSize - 100) + 50)
      this.blockX = 2
      this.blockY = 2
      this.setValue(false)
    }
  },
  mounted () {
    this.ActualWidth = this.width
    this.$nextTick(() => {
      this.resetValidArea()
    })
    document.addEventListener('click', this.func)
  },
  destroyed () {
    document.removeEventListener('click', this.func)
  },
  watch: {
    value (v) {
      this.blockX = v ? this.areaX : 0
      this.blockY = v ? this.areaY : 0
    }
  },
  computed: {
    AWidth () {
      return this.width > 300 ? this.width : 300
    },
    AHeight () {
      return this.height > 162 ? this.height : 162
    },
    aSize () {
      let min = this.AWidth - 100
      let areaSize = this.areaSize
      if (this.AHeight - 100 < min) {
        min = this.AHeight - 100
      }
      if (areaSize > min) {
        return min > 50 ? min : 50
      }
      return areaSize
    },
    style1 () {
      return {
        width: this.AWidth + 'px',
        height: this.AHeight + 'px',
        backgroundImage: 'url(' + this.imgSrc + ')',
        backgroundSize: '100% 100%'
      }
    },
    style2 () {
      return {
        width: this.aSize + 'px',
        height: this.aSize + 'px',
        left: this.blockX + 'px',
        top: this.blockY + 'px',
        backgroundImage: 'url(' + this.imgSrc + ')',
        backgroundSize: this.AWidth + 'px ' + this.AHeight + 'px',
        backgroundPosition: -this.areaX + 'px ' + -this.areaY + 'px',
        transition: this.value ? 'all 0.2s' : 'none'
      }
    },
    style3 () {
      return {
        width: this.aSize + 'px',
        height: this.aSize + 'px',
        left: this.areaX + 'px',
        top: this.areaY + 'px'
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.image-verify {
  .model {
    top: 0;
    left: 0px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
  }
  .content {
    position: relative;
    z-index: 10;
    overflow: hidden;
    border-radius: 4px;
    display: inline-block;
    border: 1px solid #aaa;
  }
  .entire-image {
    background-repeat: no-repeat;
    background-color: #fff;
  }
  .valid-area {
    position: absolute;
    z-index: 11;
    background-color: rgba(255, 255, 255, 0.6);
    border-radius: 6px;
    box-shadow: #888 0px 0px 4px inset;
    border: 1px solid #c7c7c7;
  }
  .valid-block {
    position: absolute;
    z-index: 12;
    overflow: hidden;
    border-radius: 6px;
    box-shadow: #c7c7c7 0px 0px 4px inset;
    border: 1px solid #888;
    background-repeat: no-repeat;
    background-color: #fff;
  }
  .message-div {
    position: absolute;
    z-index: 13;
    top: 0;
    color: #ffffff;
    font-size: 16px;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.2);
    user-select: none;
  }
  .shade {
    position: absolute;
    z-index: 12;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    opacity: 0.5;
    background: #000000;
  }
  .btn {
    display: flex;
    justify-content: space-between;
    border: 1px solid #ddd;
    border-radius: 5px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
    user-select: none;
    background-color: #fff;
    > div {
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      flex: 1;
      height: 38px;
      &:not(:first-child) {
        border-left: 1px solid #ddd;
      }
    }
  }
}
</style>
