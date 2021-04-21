/**
 * Skeleton
 * Created by Jervis
 * 2021-04-19
 * @prop ...
 */
<template>
  <view class="Skeleton">
    <view :style="`background: ${bgColor};`" class="Skeleton-wrap" v-on:touchmove="_defaultNone" v-if="isShow">
      <!--矩形：文本、图片-->
      <view :class="`Skeleton-item Skeleton-${setPropStyle}`" v-for="item in rectList" :key="item.id"
            :style="`width: ${item.width}px; height: ${item.height}px; top: ${item.top}px; left: ${item.left}px; ${borderLine}`">
      </view>
      <!--圆形: 头像-->
      <view :class="`Skeleton-item Skeleton-${setPropStyle}`" v-for="item in cirList" :key="item.id"
            :style="`width: ${item.width}px; height: ${item.height}px; top: ${item.top}px; left: ${item.left}px; border-radius: ${item.width}px; ${borderLine}`">
      </view>
      <!-- 富文本默认图文模板 -->
      <view :class="`Skeleton-item Skeleton-${setPropStyle}`" v-for="item in richTextList" :key="item.id"
            :style="`width: ${item.width}px; height: ${item.height}px; top: ${item.top}px; left: ${item.left}px; ${borderLine}`">
      </view>
    </view>
  </view>
</template>

<script>
import { eventCenter, getCurrentInstance, createSelectorQuery, getSystemInfoSync } from '@tarojs/taro'

export default {
  data() {
    return {
      query: null,
      rectList: [],
      cirList: [],
      richTextList: []
    }
  },
  props: {
    isShow: {
      type: Boolean,
      default: true,
    },
    bgColor: { // 骨架屏背景色
      type: String,
      default: '#fff'
    },
    selects: { // 页面根元素类名
      type: String,
      default: 'Skeleton'
    },
    setStyle: {
      type: String,
      default: 'ani'
    }
  },
  computed: {
    setPropStyle() {
      return this.setStyle === 'ani' ? 'ani' : 'static'
    },
    borderLine() {
      return `border: 1px solid ${this.bgColor};`
    }
  },
  mounted() {
    eventCenter.once(getCurrentInstance().router.onReady, () => {
      this.query = createSelectorQuery()
      this._DrawRect()
      this._DrawCircle()
      this._DrawRichText()
      this.query = null
    })
  },
  methods: {
    _defaultNone(e) {
      e.stopPropagation()
      // console.log('无事发生')
    },
    _DrawRect () {
      this.query.selectAll(`.${this.selects} >>> .${this.selects}-rect`).boundingClientRect(
        res => {
          this.rectList = res
        }
      ).exec()
    },
    _DrawRichText () {
      this.query.selectAll(`.${this.selects} >>> .${this.selects}-rich`).boundingClientRect(
        res => {
          this.richTextList = res

          if (this.richTextList.length === 1) {// 获取容器剩余面积并填充
            // H5 端不支持 version、statusBarHeight、fontSizeSetting、SDKVersion
            const systomInfo = getSystemInfoSync()
            this.richTextList[0].height = systomInfo.windowHeight - this.richTextList[0].top - 10 // 保留10px 间隙
          } else {
            // 多个富文本则使用长宽为容器宽的矩形
            this.richTextList = this.richTextList.map( (v, i, a) => {
              v.height = v.width
            })
          }
        }
      ).exec()
    },
    _DrawCircle() {
      this.query.selectAll(`.${this.selects} >>> .${this.selects}-cir`).boundingClientRect(
        res => {
          this.cirList = res
        }
      ).exec()
      // wx.createSelectorQuery()
      //   .selectAll(`.${this.selects} >>> .${this.selects}-cir`)
      //   .boundingClientRect(res => {
      //   this.cirList = res
      // }).exec()
    }
  }
}
</script>

<style lang="scss">
.Skeleton {
  &-wrap {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 9998;
    overflow: hidden;
  }
  &-item{
    box-sizing: border-box;
    position: absolute;
    border-radius: 10px;
    background-color: #eee;
  }

  &-static {
    background: rgba(255, 255, 255, .3);
    background-size: 100%;
  }

  &-ani {
    background: linear-gradient(
            110deg,
            rgba(255, 255, 255, .3) 45%,
            rgba(255, 255, 255, .5) 50%,
            rgba(255, 255, 255, .3) 55%) #eee;
    background-size: 200%;
    background-position-x: 180%;
    animation: aniKey 1s linear infinite;
  }
}


@keyframes aniKey {
  to {
    background-position-x: -20%;
  }
}

</style>
