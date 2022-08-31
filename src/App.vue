<script lang="ts" setup>
  import { computed, watchEffect, reactive } from 'vue'
  import { ChangeColor } from './utils/change-color'
  import Clipboard from 'clipboard'
  import { FMessage } from 'fighting-design'
  import type { CSSProperties } from 'vue'

  const styleList = reactive({
    width: 40,
    height: 35,
    background: '#2d5af1',
    color: '#ffffff',
    darkColor: '',
    lightColor: '',
    borderRadius: 2,
    fontSize: 14,
    text: 'Button',
    borderColor: '#000000',
    borderSize: 0,
    darkSleep: 0.3,
    lightSleep: 0.3,
  })

  watchEffect(() => {
    const changeColor: ChangeColor = new ChangeColor(styleList.background)
    const dark: string = changeColor.getDarkColor(styleList.darkSleep)
    const light: string = changeColor.getLightColor(styleList.lightSleep)

    styleList.darkColor = dark
    styleList.lightColor = light
  })

  const colorList = computed((): CSSProperties => {
    return {
      '--button-hover-color': styleList.lightColor,
      '--button-active-color': styleList.darkColor,
      width: styleList.width + 'px',
      height: styleList.height + 'px',
      borderRadius: styleList.borderRadius + 'px',
      fontSize: styleList.fontSize + 'px',
      background: styleList.background,
      color: styleList.color,
      border: `${styleList.borderSize}px solid ${styleList.borderColor}`,
    }
  })

  const copyCode = (node: string): ClipboardJS => {
    FMessage({
      message: '复制成功',
      type: 'success',
      round: true,
    })
    return new Clipboard(node)
  }

  const htmlCode = computed((): string => {
    return `<button class="f-button">${styleList.text}</button>` as const
  })

  const cssCode = computed((): string => {
    return `
    .f-button {
      height: ${styleList.height}px;
      width: ${styleList.width}px;
      color: ${styleList.color};
      background: ${styleList.background};
      font-size: ${styleList.fontSize}px;
      border-radius: ${styleList.borderRadius}px;
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
      ${
        styleList.borderSize > 0
          ? `border: ${styleList.borderSize}px solid ${styleList.borderColor};`
          : ''
      }
    }

    .f-button:hover {
      background: ${styleList.lightColor};
    }

    .f-button:active {
      background: ${styleList.darkColor};
    }
    ` as const
  })
</script>

<template>
  <div id="box">
    <f-link
      href="https://github.com/Tyh2001/Button"
      target="_blank"
      state="bag"
      size="20px"
      left-icon="f-icon-attachent"
    >
      Github
    </f-link>

    <div class="button-box">
      <div class="button" :style="colorList">
        <p class="button-text">{{ styleList.text }}</p>
      </div>
    </div>

    <ul class="option">
      <li class="item">
        <span class="title">内容</span>
        <input type="text" v-model="styleList.text" />
      </li>
      <li class="item">
        <span class="title">背景色</span>
        <input type="color" v-model="styleList.background" />
      </li>
      <li class="item">
        <span class="title">边框色</span>
        <input type="color" v-model="styleList.borderColor" />
      </li>
      <li class="item">
        <span class="title">边框尺寸</span>
        <input type="range" v-model="styleList.borderSize" :max="30" />
      </li>
      <li class="item">
        <span class="title">宽度</span>
        <input type="range" v-model="styleList.width" :max="1000" />
      </li>
      <li class="item">
        <span class="title">高度</span>
        <input type="range" v-model="styleList.height" :max="180" />
      </li>
      <li class="item">
        <span class="title">圆角</span>
        <input type="range" v-model="styleList.borderRadius" :max="100" />
      </li>
      <li class="item">
        <span class="title">文字大小</span>
        <input type="range" v-model="styleList.fontSize" :max="50" :min="12" />
      </li>
      <li class="item">
        <span class="title">文字颜色</span>
        <input type="color" v-model="styleList.color" />
      </li>
      <li class="item">
        <span class="title">Hover 减淡深度</span>
        <select v-model="styleList.lightSleep">
          <option :value="0.1">0.1</option>
          <option :value="0.2">0.2</option>
          <option :value="0.3">0.3</option>
          <option :value="0.4">0.4</option>
          <option :value="0.5">0.5</option>
        </select>
      </li>

      <li class="item">
        <span class="title">Active 加深深度</span>
        <select v-model="styleList.darkSleep">
          <option :value="0.1">0.1</option>
          <option :value="0.2">0.2</option>
          <option :value="0.3">0.3</option>
          <option :value="0.4">0.4</option>
          <option :value="0.5">0.5</option>
        </select>
      </li>
      <li class="item"></li>
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

<style>
  * {
    margin: 0;
    padding: 0;
  }
</style>

<style lang="scss" scoped>
  #box {
    max-width: 850px;
    margin: 0 auto;
    padding: 30px;
    box-sizing: border-box;
    position: relative;
    .f-link {
      position: absolute;
      top: 30px;
      right: 50px;
    }

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
      display: flex;
      justify-content: space-around;
      align-items: center;
      flex-wrap: wrap;
      margin: 0 auto;

      .item {
        list-style: none;
        width: 300px;
        color: #a8abb2;
        line-height: 50px;
        display: flex;
        align-items: center;

        .title {
          display: inline-block;
          width: 190px;
          user-select: none;
        }

        input,
        select {
          width: 150px;
          padding: 0 7px;
          height: 30px;
          background: rgb(245, 245, 245);
          outline: none;
          border: 1px solid #dfe6fc;
          cursor: pointer;
          transition: 0.3s;

          &:focus {
            background: #fff;
            border: 1px solid #2d5af1;
          }
        }
      }
    }
    .result {
      width: 300px;
      margin: 0 auto;
      margin-bottom: 60px;
      margin-top: 50px;
      display: flex;
      justify-content: space-around;
      align-items: center;
    }
  }
</style>
