<script lang="ts" setup>
  import { computed, watchEffect, reactive } from 'vue'
  import { ChangeColor } from './utils/change-color'
  import Clipboard from 'clipboard'
  import { FMessage } from 'fighting-design'
  import type { CSSProperties } from 'vue'

  const styleList = reactive({
    width: 110,
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
    unit: 'px',
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
      width: styleList.width + styleList.unit,
      height: styleList.height + styleList.unit,
      borderRadius: styleList.borderRadius + styleList.unit,
      fontSize: styleList.fontSize + styleList.unit,
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
    return `<button class="demo-button">${styleList.text}</button>` as const
  })

  const cssCode = computed((): string => {
    return `
    .demo-button {
      height: ${styleList.height}${styleList.unit};
      width: ${styleList.width}${styleList.unit};
      color: ${styleList.color};
      background: ${styleList.background};
      font-size: ${styleList.fontSize}${styleList.unit};
      border-radius: ${styleList.borderRadius}${styleList.unit};
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
      padding: 0 25${styleList.unit};
      overflow: hidden;
      white-space: nowrap;
      vertical-align: middle;
      ${
        styleList.borderSize > 0
          ? `border: ${styleList.borderSize}${styleList.unit} solid ${styleList.borderColor};`
          : ''
      }
    }

    .demo-button:hover {
      background: ${styleList.lightColor};
    }

    .demo-button:active {
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
      size="20px"
      left-icon="f-icon-attachent"
    >
      Github
    </f-link>

    <!-- 演示盒子 -->
    <div class="button-box">
      <button class="button" :style="colorList">{{ styleList.text }}</button>
    </div>

    <!-- 参数配置项 -->
    <div class="option">
      <div class="item">
        <div class="label">内容</div>
        <div class="value-box">
          <f-input type="text" v-model="styleList.text" />
        </div>
      </div>

      <div class="item">
        <div class="label">整体单位</div>
        <div class="value-box">
          <f-select v-model="styleList.unit" placeholder="请选择……">
            <f-option value="px">px</f-option>
            <f-option value="em">em</f-option>
            <f-option value="rem">rem</f-option>
            <f-option value="rpx">rpx</f-option>
          </f-select>
        </div>
      </div>

      <div class="item">
        <div class="label">背景色</div>
        <div class="value-box">
          <input
            type="color"
            class="color-input"
            v-model="styleList.background"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">边框色</div>
        <div class="value-box">
          <input
            type="color"
            class="color-input"
            v-model="styleList.borderColor"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">宽度</div>
        <div class="value-box">
          <f-input-number
            v-model="styleList.width"
            :max="500"
            :min="10"
            model="switch"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">高度</div>
        <div class="value-box">
          <f-input-number
            v-model="styleList.height"
            :max="200"
            :min="10"
            model="switch"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">圆角</div>
        <div class="value-box">
          <f-input-number
            v-model="styleList.borderRadius"
            :max="100"
            :min="0"
            model="switch"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">边框尺寸</div>
        <div class="value-box">
          <f-input-number
            v-model="styleList.borderSize"
            :max="30"
            :min="0"
            model="switch"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">文字大小</div>
        <div class="value-box">
          <f-input-number
            v-model="styleList.fontSize"
            :max="50"
            :min="1"
            model="switch"
          />
        </div>
      </div>

      <div class="item">
        <div class="label">文字颜色</div>
        <div class="value-box">
          <input type="color" class="color-input" v-model="styleList.color" />
        </div>
      </div>

      <div class="item">
        <div class="label">Hover 减淡深度</div>
        <div class="value-box">
          <f-select v-model="styleList.lightSleep" placeholder="请选择……">
            <f-option :value="0.1">0.1</f-option>
            <f-option :value="0.2">0.2</f-option>
            <f-option :value="0.3">0.3</f-option>
            <f-option :value="0.4">0.4</f-option>
            <f-option :value="0.5">0.5</f-option>
          </f-select>
        </div>
      </div>

      <div class="item">
        <div class="label">Active 加深深度</div>
        <div class="value-box">
          <f-select v-model="styleList.darkSleep" placeholder="请选择……">
            <f-option :value="0.1">0.1</f-option>
            <f-option :value="0.2">0.2</f-option>
            <f-option :value="0.3">0.3</f-option>
            <f-option :value="0.4">0.4</f-option>
            <f-option :value="0.5">0.5</f-option>
          </f-select>
        </div>
      </div>
    </div>

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

<style lang="scss">
  * {
    margin: 0;
    padding: 0;
  }

  body {
    &::-webkit-scrollbar {
      width: 0;
    }

    &::-webkit-scrollbar-thumb,
    &::-webkit-scrollbar-track {
      background-color: transparent;
    }
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
      width: 100%;
      display: grid;
      grid-column-gap: 60px;
      grid-row-gap: 16px;
      grid-template-columns: repeat(2, 1fr);

      .item {
        color: #a8abb2;
        line-height: 50px;
        display: flex;
        align-items: center;
        justify-content: space-between;

        .label {
          display: flex;
          user-select: none;
          align-items: center;
          flex: 1;
          flex-shrink: 0;
        }

        .value-box {
          flex: 1;
          flex-shrink: 0;
          display: flex;
          align-items: center;

          .color-input {
            width: 190px;
            height: 35px;
            border-radius: 5px;
          }
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
