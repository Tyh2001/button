<script lang="ts" setup>
  import { reactive, ref, watch, computed } from 'vue'
  import { ChangeColor } from './utils/change-color'
  import { debounce } from './utils/utils'

  const width = ref(40)
  const background = ref('#2d5af1')
  const darkColor = ref('')
  const lightColor = ref('')

  const addWidth = () => {
    width.value += 3
  }

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
</script>

<template>
  <f-card>
    <div
      class="button"
      :style="{ width: width + 'px', background, ...colorList }"
    >
      <p class="button-text" contenteditable="true">123</p>
    </div>
    <br />
    <f-button @click="addWidth">增加长度</f-button>
    <input type="color" v-model="background" />
  </f-card>
</template>

<style lang="scss" scoped>
  * {
    margin: 0;
    padding: 0;
  }
  .f-card {
    width: 900px;
    margin: auto;
    margin-top: 50px;
    position: relative;
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
</style>
