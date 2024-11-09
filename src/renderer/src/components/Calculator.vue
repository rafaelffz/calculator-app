<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

const expression = ref<string>('')

const allowedValues: string[] = [
  '0',
  '1',
  '2',
  '3',
  '4',
  '5',
  '6',
  '7',
  '8',
  '9',
  '+',
  '-',
  '/',
  '*',
  '=',
  '.',
  'C',
  'Enter',
  'Backspace',
  'Delete'
]

const calculateExpression = (value: string): void => {
  if (value) {
    if (value === 'C') {
      expression.value = ''
    } else if (value === '=' || value === 'Enter') {
      expression.value = `${eval(expression.value)}`
    } else if (value == 'Backspace' || value === 'Delete') {
      expression.value = expression.value.substring(0, expression.value.length - 1)
    } else {
      expression.value += value
    }
  }
}

const handleButtonClick = (event: MouseEvent) => {
  const target = event.target as HTMLElement
  const value = target.dataset.value

  if (value) calculateExpression(value)
}

const handleKeyDown = (event: KeyboardEvent) => {
  const value = event.key
  if (value === 'Enter') {
    event.preventDefault()
  }

  if (allowedValues.includes(value)) {
    calculateExpression(value)
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>

<template>
  <div class="container" @click.prevent="handleButtonClick($event)">
    <div class="result-container">
      <span class="result">{{ expression }}</span>
    </div>

    <div class="row">
      <button data-value="C">C</button>
      <button data-value="%">%</button>
      <button data-value="Backspace">&#x021D0;</button>
      <button data-value="/">/</button>
    </div>

    <div class="row">
      <button data-value="7">7</button>
      <button data-value="8">8</button>
      <button data-value="9">9</button>
      <button data-value="*">*</button>
    </div>

    <div class="row">
      <button data-value="4">4</button>
      <button data-value="5">5</button>
      <button data-value="6">6</button>
      <button data-value="+">+</button>
    </div>

    <div class="row">
      <button data-value="1">1</button>
      <button data-value="2">2</button>
      <button data-value="3">3</button>
      <button data-value="-">-</button>
    </div>

    <div class="row">
      <button data-value="**">**</button>
      <button data-value="0">0</button>
      <button data-value=".">.</button>
      <button data-value="=">=</button>
    </div>
  </div>
</template>

<style scoped></style>
