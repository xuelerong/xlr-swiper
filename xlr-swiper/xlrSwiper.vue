<template>
    <div id="xlrWrapper">
        <div id="xlrItems" @touchstart="startContainer" @touchmove="moveContaine" @touchend="endContaine">
            <slot></slot>
        </div>
        <div id="indicators" v-if="playIndicator" v-show="this.banner.length > 0">
            <span @touchstart="touchstart(index)" @touchend="touchEnd()" :class="{active:isActive === index}"
                  v-for="(item,index) in banner" :key="index"></span>
        </div>
    </div>
</template>
<script>
  export default {
    name: "xlrSwiper",
    props: {
      banner: {
        type: Array,
        default() {
          return []
        }
      },
      timeout: {
        type: Number,
        default: 2000
      },
      playIndicator: {
        type: Boolean,
        default: false
      },
      clickIndicator: {
        type: Boolean,
        default: false
      },
      speedImg: {
        type: Number,
        default: 6
      },
      pullLenght: {
        type: Number,
        default: 0
      },
      openPull: {
        type: Boolean,
        default: false
      },
      autoSwiper: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        index: 1,
        xlrSwipeLength: 0,
        isActive: 0,
        timer: null,
        isTime: false,
        wrappers: null,
        wrappersWidth: 0,
        coordinateX: 0,
        swiperLeft: 0,
        dataSpeedImg: this.speedImg,
        dataPullLenght: this.pullLenght
      }
    },
    mounted() {
      setTimeout(() => {
        if (this.banner.length > 0)
          this.xSwiper()
        this.isTime = true
      }, 1000)
    },
    methods: {
      xSwiper() {
        this.wrappers = document.getElementById("xlrItems")
        this.wrappersWidth = parseInt(getComputedStyle(this.wrappers, null).width)
        this.xlrSwipeLength = this.wrappers.childNodes.length
        this.wrappers.style.left = -this.wrappersWidth + "px"
        let wrappersfistPic = this.wrappers.firstElementChild.cloneNode(true)
        this.wrappers.appendChild(wrappersfistPic)
        let wrapperslastPic = this.wrappers.childNodes[this.xlrSwipeLength - 1].cloneNode(true)
        this.wrappers.insertBefore(wrapperslastPic, this.wrappers.firstElementChild)
        if (this.autoSwiper) {
          this.timer = setInterval(() => {
            this.index++;
            this.isActive = this.index - 1
            this.getXlrSwipers()
          }, this.timeout)
        }
      },
      touchstart(e) {
        if (this.clickIndicator) {
          clearInterval(this.timer)
          this.index = e + 1
          this.isActive = this.index - 1
          this.getXlrSwipers()
        }
      },
      touchEnd() {
        if (this.clickIndicator && this.autoSwiper) {
          this.timer = setInterval(() => {
            this.isActive = this.index
            this.getXlrSwipers()
            this.index++
          }, this.timeout)
        }
      },
      getXlrSwipers() {
        let time
        time = setInterval(() => {
          let wrapperleft = 0;
          wrapperleft = parseInt(getComputedStyle(this.wrappers, null).left)
          this.dataSpeedImg = this.dataSpeedImg > 10 ? this.dataSpeedImg = 10 : this.dataSpeedImg
          this.dataSpeedImg = this.dataSpeedImg < 1 ? this.dataSpeedImg = 1 : this.dataSpeedImg
          let speed = (-this.wrappersWidth * this.index - wrapperleft) / this.dataSpeedImg
          speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed)
          this.wrappers.style.left = wrapperleft + speed + "px"
          if (this.wrappers.style.left === this.index * -this.wrappersWidth + "px") {
            clearInterval(time, time = null)
          }
          if (this.index === this.xlrSwipeLength + 1) this.isActive = 0
          if (this.index === this.xlrSwipeLength + 1 && time === null) {
            this.index = 1
            this.wrappers.style.left = -this.wrappersWidth + "px"
          }
          if (this.index === 0) {
            this.isActive = this.xlrSwipeLength - 1
          }
          if (this.index === 0 && time === null) {
            this.index = this.xlrSwipeLength
            this.wrappers.style.left = -this.wrappersWidth * this.index + "px"
          }
        }, 10)
      },
      startContainer(e) {
        if (this.openPull && this.isTime) {
          clearInterval(this.timer)
          this.coordinateX = e.changedTouches[0].pageX;
          this.swiperLeft = this.wrappers.offsetLeft;
        }
      },
      moveContaine(e) {
        if (this.openPull && this.isTime) {
          let slideX = e.changedTouches[0].pageX;
          let disX = slideX - this.coordinateX;
          let left = this.swiperLeft + disX;
          this.wrappers.style.left = left + 'px';
        }
      },
      endContaine() {
        if (this.openPull && this.isTime) {
          let left = this.wrappers.offsetLeft;
          this.dataPullLenght = this.dataPullLenght > 40 ? this.dataPullLenght = 40 : this.dataPullLenght
          this.dataPullLenght = this.dataPullLenght < 0 ? this.dataPullLenght = 0 : this.dataPullLenght
          left < -this.wrappersWidth * this.index ?
              this.index = Math.round(-left / (this.wrappersWidth - this.dataPullLenght)) :
              this.index = Math.round(-left / (this.wrappersWidth + this.dataPullLenght));
          this.isActive = this.index - 1
          this.getXlrSwipers()
          if (this.autoSwiper) {
            this.timer = setInterval(() => {
              this.isActive = this.index
              this.getXlrSwipers()
              this.index++
            }, this.timeout)
          }
        }
      }
    }
  }
</script>

<style scoped>
    #xlrWrapper {
        position: relative;
        width: 100%;
        overflow: hidden;
    }

    #xlrItems {
        position: relative;
        width: 100%;
        display: flex;

    }

    #indicators {
        position: absolute;
        height: 0.48rem;
        line-height: 0.48rem;
        z-index: 1000;
        left: 50%;
        bottom: 0.05rem;
        background-color: #bbbde1;
        opacity: 1;
        border-radius: 0.4rem;
        padding: 0 0.08rem;
        opacity: 0.8;
        transform: translate(-50%, -50%);
    }

    #indicators span {
        display: inline-block;
        margin-left: 0.1rem;
        margin-right: 0.1rem;
        width: 0.2rem;
        height: 0.2rem;
        border-radius: 50%;
        background-color: white;
    }

    .active {
        background-color: red !important;
    }

</style>
