<script setup>
import { defineExpose, ref } from 'vue'

const error = ref(null)

defineExpose({ error })


let opening = ref([])
let wallWidth = ref(5)
let wallHeight = ref(2.7)
let pgpCount = ref(0)
let weight = ref(0)
let wallSquare = ref(0)
let openingSquareSum = ref(0)
let pgpWidth = 0.667
let pgpHeight = 0.5
let pgpSqare = 0.3335
let pgpWeight = 36


let deleteOpening = id => {
  opening.value.splice(id, 1)
}
let calculate = () => {
  error.value.style.display = "none"
  wallSquare.value = wallWidth.value * wallHeight.value
  let openingSquare = []

  if (opening.value.length) {
    openingSquare = opening.value.map(el => (el[0] / 100) * (el[1] / 100))
    openingSquareSum.value = openingSquare.reduce((sum, x) => sum + x)
  }
  if (openingSquareSum.value > wallSquare.value) {
    error.value.style.display = "block"

    return
  }
  pgpCount.value = Math.ceil((wallSquare.value - openingSquareSum.value) / pgpSqare)
  weight.value = pgpCount.value * pgpWeight

}

</script>

<template>
  <main>
    <h1>Расчет перегородок из пазогребневых плит</h1>
    <div>
      <label for="wallWidth">Введите общую длину стены-перегородки в метрах погонных:</label>
      <input v-model="wallWidth" type="number" id="wallWidth" name="wallWidth" />
    </div>
    <div>
      <label for="wallHeight">Введите высоту её в метрах:</label>
      <input v-model="wallHeight" type="number" id="wallHeight" name="wallHeight" />
    </div>
    <div>
      <div>
        <strong>Проёмы (для дверей или других целей):</strong>
      </div>
      <button @click="opening.push([90, 205])">Добавить проем</button>
      <div v-for="(item, i) in opening" :key="i">
        <label for="openingWidth">Ширина</label>
        <input v-model="item[0]" type="number" id="openingWidth" name="openingWidth" />
        <label for="openingHeight">Высота</label>
        <input v-model="item[1]" type="number" id="openingHeight" name="openingHeight" />
        <button @click="deleteOpening(i)">Х</button>
      </div>
    </div>
    <button class="calculate" @click="calculate">Считать</button>
    <div ref="error" class="error">
      <p>Площадь проёмов {{ openingSquareSum }} м.кв. больше площади стены {{ wallSquare }} м.кв. !</p>
    </div>
    <div class="result">
      <div>Пазогребневых гипсовых блоков: {{ pgpCount }}</div>
      <div>Вес всех плит: {{ weight }}</div>
    </div>
  </main>
</template>

<style scoped>
.error {
  display: none;
  color: red;
}
main {
  margin: 0 10%;
  padding: 20px 0;
  background-color: rgb(195, 240, 195);
}
div {
  margin-top: 10px;
}
button {
  height: 25px;
  border-radius: 5px;
  margin-top: 20px;
}
.calculate {
  font-size: 20px;
  height: auto;
}
input {
  width: 60px;
  height: 10px;
  font-size: 15px;
  margin: 5px;
  padding: 5px;
  border-radius: 5px;
}
.result {
  color: rgb(72, 17, 202);
}
</style>
