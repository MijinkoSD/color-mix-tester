<script setup lang="ts">
import { computed, ref } from 'vue'

const leftColor = ref('red')
const rightColor = ref('blue')
const _selectedColorSpace = ref('srgb')
const selectedColorSpace = computed({
  get: () => _selectedColorSpace.value,
  set: (value: string) => {
    _selectedColorSpace.value = value
    if (!isEnableHueInterpolation(value)) {
      selectedHueInterpolationMethod.value = ''
    }
  }
})
const selectedHueInterpolationMethod = ref('')

const rectangularColorSpaces = [
  'srgb',
  'srgb-linear',
  'display-p3',
  'a98-rgb',
  'prophoto-rgb',
  'rec2020',
  'lab',
  'oklab',
  'xyz',
  'xyz-d50',
  'xyz-d65'
]
const polarColorSpaces = ['hsl', 'hwb', 'lch', 'oklch']
const colorSpaces = rectangularColorSpaces.concat(polarColorSpaces)
const hueInterpolationMethods = ['shorter hue', 'longer hue', 'increasing hue', 'decreasing hue']

/**
 * 色相補完のアルゴリズムを選択可能にするか（極座標の色空間であるかどうか）を返す
 * @param {string} colorSpace 色空間
 * @returns {boolean} trueで選択可能
 */
const isEnableHueInterpolation = (colorSpace = selectedColorSpace.value) => {
  return polarColorSpaces.includes(colorSpace)
}

/**
 * linear-gradient()で使用する文字列を返す
 */
const getLinearGradientString = () => {
  return (
    `linear-gradient(90deg, ${leftColor.value}, ` +
    `color-mix(in ${selectedColorSpace.value} ${selectedHueInterpolationMethod.value}, ` +
    `${leftColor.value}, ${rightColor.value}), ${rightColor.value})`
  )
}
</script>

<template>
  <div class="tester">
    <div class="settings">
      <div class="left color input">
        <label>Left Color</label>
        <input type="text" v-model="leftColor" />
      </div>
      <div class="color-space input">
        <label>Color space</label>
        <select v-model="selectedColorSpace">
          <option v-for="colorSpace in colorSpaces" :key="colorSpace" :value="colorSpace">
            {{ colorSpace }}
          </option>
        </select>
      </div>
      <div class="hue input">
        <label>Hue interpolation method</label>
        <select v-model="selectedHueInterpolationMethod" :disabled="!isEnableHueInterpolation()">
          <option value=""></option>
          <option v-for="method in hueInterpolationMethods" :key="method" :value="method">
            {{ method }}
          </option>
        </select>
      </div>
      <div class="right color input">
        <label>Right Color</label>
        <input type="text" v-model="rightColor" />
      </div>
    </div>
    <div
      class="display"
      :style="{
        background: getLinearGradientString()
      }"
    ></div>
  </div>
</template>

<style scoped>
.tester {
  display: flex;
  flex-direction: column;
  width: fit-content;
  gap: 0.5em;
}

.settings {
  display: flex;
  flex-direction: row;
  gap: 0.5em;

  .input {
    display: flex;
    flex-direction: column;
  }
}

.display {
  width: 100%;
  height: 60px;
}
</style>
