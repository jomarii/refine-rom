<template>
    <div class="flex flex-col border border-gray-300 rounded-md p-4 space-x-4">
        <div class="m-auto flex flex-row gap-2 mb-10">
            <div class="flex flex-row gap-2">
                <span class="text-gray-500 my-auto">Zeny</span>
                <input class="border border-gray-300 rounded-md p-2 outline-none" type="text" v-model="zeny" />
            </div>
            <div class="flex flex-row gap-2">
                <span class="text-gray-500 my-auto">Equipment Price</span>
                <input class="border border-gray-300 rounded-md p-2 outline-none" type="text" v-model="equipment" />
            </div>
        </div>
        <div class="flex flex-col gap-2 m-auto">
            <div class="flex gap-2">
                <div class="rounded-full w-30 h-30 flex" :class="resultClass">
                    <span class="m-auto text-4xl">{{ refineCount }}</span>
                </div>
            </div>
            <button class="border text-white rounded-md p-2 cursor-pointer" :class="isDisabled ? 'bg-gray-300 border-gray-300' : 'bg-amber-400 border-amber-500'" @click="refine" v-if="!isBroken" :disabled="isDisabled">Refine</button>
            <button class="bg-red-400 text-white rounded-md p-2 border border-red-500 cursor-pointer" @click="repair" v-else>Repair</button>
        </div>
    </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const refineCount = ref(0)
const zeny = ref(1000000)
const equipment = ref(0)
const zenyPerRefine = ref([
    10000,
    20000,
    30000,
    40000,
    50000,
    60000,
    70000,
    80000,
    90000,
    100000,
    100000,
    100000,
    100000,
    100000,
    100000
])

const isBroken = ref(false)

const result = ref(null)

const resultClass = computed(() => {
    if(result.value == null || result.value == 'repaired') return 'bg-gray-300'
    if(result.value == 'success' && refineCount.value < 15) return 'bg-green-400'
    if(result.value == 'success' && refineCount.value == 15) return 'bg-blue-400'
    if(result.value == 'broken') return 'bg-red-700'
    return 'bg-red-300'
})

const isDisabled = computed(() => {
    return zeny.value < zenyPerRefine.value[refineCount.value] || refineCount.value == 15
})

const successRate = computed(() => {
    return refineCount.value > 5 ? 0.4 : 0.5
})

const breakRate = computed(() => {
    return refineCount.value > 5 ? 0.1 : 0.2
})

const refine = () => {
    if(refineCount.value == 15){
        return
    }

    zeny.value -= zenyPerRefine.value[refineCount.value]

    if(refineCount.value < 4){
        result.value = 'success'
        refineCount.value++
        return
    }
    
    const success = Math.random() < successRate.value
    console.log(`Success Rate: ${successRate.value}`)
    if(isDisabled.value){
        return
    }

    if (success) {
        result.value = 'success'
        refineCount.value++
        return
    }
    
    refineCount.value--

    console.log(`Break Rate: ${breakRate.value}`)
    const isBreak = Math.random() < breakRate.value
    if(isBreak){
        isBroken.value = true
        result.value = 'broken'
        return
    }

    result.value = 'failed'
}

const repair = () => {
    zeny.value -= equipment.value
    isBroken.value = false
    result.value = 'repaired'
}
</script>