<script setup>
import { computed, ref } from 'vue'
import trepte from '@/assets/tabelTrepte.json'
import bg from '@/assets/bg2.jpg'

const dnom = ref(0)
const ei = ref(0)
const es = ref(0)

const toleranta = computed(() => {
  if (es.value < es.value) {
    return 'Abaterea superioara trebuie sa fie mai mare decat abaterea inferioara'
  }
  return es.value - ei.value
})

const treaptaDePrecizie = computed(() => {
  const toleranta = es.value - ei.value

  if (dnom.value < 60 || dnom.value > 100) {
    return 'Dimensiunea nominala trebuie sa fie intre 60 si 100'
  }

  if (dnom.value < 80) {
    const treapta = trepte[0].find((treapta) => {
      return treapta.tol == toleranta
    })

    if (treapta) {
      return treapta['treapta']
    }
    return 'Nu exista treapta pentru aceasta toleranta'
  } else if (dnom.value < 100) {
    const treapta = trepte[1].find((treapta) => {
      return treapta.tol == toleranta
    })

    if (treapta) {
      return treapta['treapta']
    }
    return 'Nu exista treapta pentru aceasta toleranta'
  }
  return es.value - ei.value
})
</script>

<template>
  <div :style="`background-image:url(${bg}) ;`" class="page">
    <h1>Calulator treapta de precizie (60mm - 100mm):</h1>
    <div class="container">
      <form @submit.prevent="">
        <div class="dnom">
          <label for="dnom">Dimensiunea Nominala: </label>
          <input id="dnom" type="number" v-model="dnom" />
        </div>

        <div class="abateri">
          <div style="width: 100%">
            <label for="ei">Abaterea inferioara: </label>
            <input id="ei" type="number" v-model="ei" />
          </div>
          <div style="width: 100%">
            <label for="es">Abaterea superioara: </label>
            <input id="es" type="number" v-model="es" />
          </div>
        </div>
      </form>

      <div class="rezultate">
        <h3>Toleranta fundamentala: {{ toleranta }}</h3>
        <h3>Treapta de precizie: {{ treaptaDePrecizie }}</h3>
      </div>
    </div>
  </div>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
}
input {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  background-color: #f1f1f1;
  border-radius: 5px;
  border: 1px solid black;
}

.rezultate {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
  padding: 20px;
  border: 1px solid black;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border-radius: 10px;
  width: 50%;
  height: 300px;
}

.abateri {
  display: flex;
  width: 100%;
  flex-direction: row;
  margin-bottom: 20px;
  gap: 20px;
}

.dnom {
  width: 100%;
}

.container {
  width: 80%;
  display: flex;
  flex-direction: row;
  gap: 60px;
  justify-content: center;
  align-items: center;
}

.page {
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-size: cover;
}
</style>
