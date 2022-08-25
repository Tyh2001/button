<script lang="ts" setup>
  import { ref, watch, computed } from 'vue'
  import { ChangeColor } from './utils/change-color'
  import { debounce } from './utils/utils'
  import Clipboard from 'clipboard'
  import { FMessage } from 'fighting-design'

  const width = ref(40)
  const height = ref(35)
  const background = ref('#2d5af1')
  const color = ref('#ffffff')
  const darkColor = ref('')
  const lightColor = ref('')
  const borderRadius = ref(2)
  const fontSize = ref(14)

  watch(
    () => background.value,
    debounce(() => {
      const changeColor: ChangeColor = new ChangeColor(background.value)
      const dark: string = changeColor.getDarkColor(0.3)
      const light: string = changeColor.getLightColor(0.3)

      darkColor.value = dark
      lightColor.value = light
    }),
    { immediate: true }
  )

  const colorList = computed(() => {
    return {
      '--button-hover-color': lightColor.value,
      '--button-active-color': darkColor.value,
    }
  })

  const copyCode = (node: string): void => {
    FMessage({
      message: '复制成功',
      type: 'success',
      round: true,
    })
    return new Clipboard(node)
  }
  const htmlCode = computed(() => {
    return `<button class="f-button">你好啊</button>`
  })
  const cssCode = computed(() => {
    return `
    .f-button {
      height: ${height.value}px;
      width: ${width.value}px;
      color: ${color.value};
      background: ${background.value};
      font-size: ${fontSize.value}px;
      border-radius: ${borderRadius.value}px;
      display: inline-flex;
      justify-content: center;
      align-items: center;
      border: none;
      outline: none;
      user-select: none;
      text-decoration: none;
      transition: 0.3s;
      line-height: 1;
      cursor: pointer;
      padding: 0 25px;
      overflow: hidden;
      white-space: nowrap;
      vertical-align: middle;
    }

    .f-button:hover {
      background: ${lightColor.value};
    }

    .f-button:active {
      background: ${darkColor.value};
    }
    `
  })
</script>

<template>
  <div id="box">
    <div class="button-box">
      <div
        class="button"
        :style="{
          width: width + 'px',
          height: height + 'px',
          borderRadius: borderRadius + 'px',
          fontSize: fontSize + 'px',
          background,
          color,
          ...colorList,
        }"
      >
        <p class="button-text" contenteditable="true">123</p>
      </div>
    </div>

    <ul class="option">
      <li class="item">
        <span class="title">背景色</span>
        <input type="color" v-model="background" />
      </li>
      <li class="item">
        <span class="title">宽度</span>
        <input type="range" v-model="width" :max="1000" />
      </li>
      <li class="item">
        <span class="title">高度</span>
        <input type="range" v-model="height" :max="100" />
      </li>
      <li class="item">
        <span class="title">圆角</span>
        <input type="range" v-model="borderRadius" :max="100" />
      </li>
      <li class="item">
        <span class="title">文字大小</span>
        <input type="range" v-model="fontSize" :max="50" :min="12" />
      </li>
      <li class="item">
        <span class="title">文字颜色</span>
        <input type="color" v-model="color" />
      </li>
    </ul>

    <div class="result">
      <f-button
        class="html"
        round
        simple
        :data-clipboard-text="htmlCode"
        @click="copyCode('.html')"
      >
        复制 HTML
      </f-button>
      <f-button
        class="css"
        round
        simple
        :data-clipboard-text="cssCode"
        @click="copyCode('.css')"
      >
        复制 CSS
      </f-button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
  * {
    margin: 0;
    padding: 0;
  }

  #box {
    width: 850px;
    margin: 0 auto;
    padding: 30px;
    box-sizing: border-box;
    .button-box {
      height: 200px;
      display: flex;
      justify-content: center;
      align-items: center;
      .button {
        height: 35px;
        color: #fff;
        font-size: 14px;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        font-size: 14px;
        border: none;
        outline: none;
        user-select: none;
        text-decoration: none;
        transition: 0.3s;
        line-height: 1;
        cursor: pointer;
        padding: 0 25px;
        overflow: hidden;
        white-space: nowrap;
        vertical-align: middle;
        &:hover {
          background: var(--button-hover-color) !important;
        }
        &:active {
          background: var(--button-active-color) !important;
        }
      }
    }
    .option {
      width: 350px;
      margin: 0 auto;

      .item {
        list-style: none;
        color: #a8abb2;
        line-height: 40px;
        display: flex;
        align-items: center;

        .title {
          display: inline-block;
          width: 190px;
        }
      }
    }
    .result {
      width: 300px;
      margin: 0 auto;
      margin-top: 50px;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }
  }
</style>
