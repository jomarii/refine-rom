<template>
    <div class="flex flex-col gap-2 mt-10 border border-gray-300 rounded-md p-4 bg-white">
      <div class="flex flex-row gap-2 mb-10">
          <div class="flex flex-row gap-2">
              <span class="text-gray-500 my-auto">Zeny</span>
              <input class="border border-gray-300 rounded-md p-2 outline-none" type="text" v-model="zeny" />
          </div>
          <div class="flex flex-row gap-2">
              <span class="text-gray-500 my-auto">Main Eq Price</span>
              <input class="border border-gray-300 rounded-md p-2 outline-none" type="text" v-model="mainEq" />
          </div>
          <div class="flex flex-row gap-2">
              <span class="text-gray-500 my-auto">Alay Eq Price</span>
              <input class="border border-gray-300 rounded-md p-2 outline-none" type="text" v-model="alayEq" />
          </div>
          <div class="flex flex-row gap-2">
              <span class="text-gray-500 my-auto">Expense {{ formattedExpense }}</span>
          </div>
      </div>
      <div class="flex flex-row space-x-10">
          <div class="flex flex-col gap-2">
              <span class="text-gray-500 my-auto">Main</span>
              <Refine :zeny="parseInt(zeny)" :equipment="parseInt(mainEq)" @update:zeny="updateZeny" @update:expense="updateExpense" @update:eqRepair="updateMainEqRepair" />
          </div>
          <div class="flex flex-col gap-2">
              <span class="text-gray-500 my-auto">Alay</span>
              <Refine :zeny="parseInt(zeny)" :equipment="parseInt(alayEq)" @update:zeny="updateZeny" @update:expense="updateExpense" @update:eqRepair="updateAlayEqRepair" />
          </div>
      </div>
      <div class="border border-gray-300 rounded-md p-4">
        <div class="flex flex-row gap-2">
            <span class="text-gray-500 my-auto">Main Repair {{ mainEqRepair }}</span>
            <span class="text-gray-500 my-auto">Alay Repair {{ alayEqRepair }}</span>
        </div>
        <div class="flex flex-row gap-2">
            
        </div>
      </div>
    </div>
</template>

<script setup>
import Refine from './components/Refine.vue'

import { ref, computed } from 'vue'

const zeny = ref(1000000)
const expense = ref(0)

const mainEq = ref(0)
const alayEq = ref(0)
const mainEqRepair = ref(0)
const alayEqRepair = ref(0)

const updateZeny = (newZeny) => {
    zeny.value = newZeny
}

const updateExpense = (exp) => {
    expense.value += parseInt(exp)
}

const formattedExpense = computed(() => {
      return new Intl.NumberFormat('en-US').format(expense.value);
})

const updateMainEqRepair = (newMainEqRepair) => {
    mainEqRepair.value += parseInt(newMainEqRepair)
}

const updateAlayEqRepair = (newAlayEqRepair) => {
    alayEqRepair.value += parseInt(newAlayEqRepair)
}
</script>
