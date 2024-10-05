<script setup>
import { ref, computed } from 'vue'

const peso = ref(0)

const unidad = ref(true)

const multiplicador = ref(1)

const arrayPesos = ref([])

const addPeso = () => {
    arrayPesos.value.push({
        valor: peso.value,
        id: new Date().getTime(),
        unidad: unidad.value ? 'kg' : 'lb',
        multiplicador: multiplicador.value
    })
    peso.value = 0
    multiplicador.value = 1
}


const pesoTotal = computed(() => {
    return arrayPesos.value.reduce((acc, peso) => {
        const valorConMultiplicador = peso.valor * peso.multiplicador;
        return peso.unidad === 'kg' ? acc + valorConMultiplicador : acc + valorConMultiplicador * 0.453592
    }, 0)
})

const toggleMultiplicador = (pesoItem) => {
    pesoItem.multiplicador = pesoItem.multiplicador === 8 ? 1 : pesoItem.multiplicador + 1;
}
</script>

<template>
    <h2>Weight Calculator</h2>
    <div style="display: flex;justify-content: space-between;padding: 0%;gap: 2.5%;">
        <div class="input-wrapper">
            <input type="number" v-model="peso" placeholder="Ingresa el peso!" />
            <button @click="unidad = !unidad">{{ unidad ? 'kg' : 'lb' }}</button>
        </div>
        <button @click="addPeso">
            +
        </button>
    </div>
    <div style="display: flex; justify-content: center;padding: 0%;">
        <ul v-if="arrayPesos.length">
            <li v-for="peso in arrayPesos" :key="peso.id">
                <button @click="toggleMultiplicador(peso)">x{{ peso.multiplicador }}</button>
                <span style="width: 100%;">{{ peso.valor }} {{ peso.unidad }}</span>
                <button @click="arrayPesos = arrayPesos.filter(p => p.id !== peso.id)">x</button>
            </li>
            <h3>Peso total {{ Math.round(pesoTotal*100)/100 }} kgs</h3>
        </ul>
    </div>
</template>

<style scoped>
button {
    border: none;
    border-radius: 50%;
    width: 44px;
    height: 40px;
    color: white;
    font-size: 1.1rem;
    cursor: pointer;
}

.input-wrapper {
    display: flex;
    justify-content: space-between;
    direction: rtl;
    width: 100%;
    position: relative;
    align-items: center;
}

.input-wrapper input {
    width: 100%;
    padding-left: 1rem;
    border-radius: 21px;
    border: none;
    height: 40px;
    direction: ltr;
    background-color: rgb(16, 16, 16);
}

.input-wrapper input:focus {
    outline: none;
}

.input-wrapper button {
    padding: 5px;
    border-radius: 50%;
    border: none;
    background-color: #242424;
    position: absolute;
    height: 35px;
    width: 35px;
    right: 4px;
}

ul {
    list-style: none;
    padding: 10px;
    width: 100%;
    border-radius: 21px;
    background-color: rgb(16, 16, 16);
    display: flex;
    flex-direction: column;
    gap: 10px;
}

li {
    display: flex;
    justify-content: space-between;
    border-radius: 20px;
    height: 35px;
    align-items: center;
    padding: 0 3px;
    background-color: #242424;
}

li button {
    height: 30px;
    width: 32px;
    text-align: center;
}
</style>