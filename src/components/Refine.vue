<template>
    <div class="flex flex-col border border-gray-300 rounded-md p-4 space-x-4">
        
        <div class="flex flex-col gap-2 m-auto">
            <div class="flex gap-2">
                <div class="rounded-full w-30 h-30 flex" :class="resultClass">
                    <span class="m-auto text-4xl">{{ refineCount }}</span>
                </div>
            </div>
            <button class="border text-white rounded-md p-2 cursor-pointer" :class="isDisabledRefine ? 'bg-gray-300 border-gray-300' : 'bg-amber-400 border-amber-500'" @click="refine" v-if="!isBroken" :disabled="isDisabledRefine">Refine</button>
            <button class="text-white rounded-md p-2 border cursor-pointer" :class="isDisabledRepair ? 'bg-gray-300 border-gray-300' : 'bg-red-400 border-red-500'" @click="repair" v-else :disabled="isDisabledRepair">Repair</button>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, reactive } from 'vue'

const emit = defineEmits(['update:zeny', 'update:expense', 'update:mainEqRepair', 'update:eqRepair'])

const props = defineProps({
    zeny: {
        type: Number,
        required: true
    },
    equipment: {
        type: Number,
        required: true
    }
})

const refineCount = ref(0)

const refineRating = reactive({
    1: {
        success: 1,
        failed: 0,
        break: 0
    },
    2: {
        success: 1,
        failed: 0,
        break: 0
    },
    3: {
        success: 1,
        failed: 0,
        break: 0
    },
    4: {
        success: 0.51,
        failed: 0.28,
        break: 0.21
    },
    5: {
        success: 0.49,
        failed: 0.25,
        break: 0.26
    },
    6: {
        success: 0.39,
        failed: 0.29,
        break: 0.32
    },
    7: {
        success: 0.42,
        failed: 0.30,
        break: 0.28
    },
    8: {
        success: 0.46,
        failed: 0.28,
        break: 0.26
    },
    9: {
        success: 0.41,
        failed: 0.31,
        break: 0.28
    },
    10: {
        success: 0.40,
        failed: 0,
        break: 0.60
    },
    11: {
        success: 0.40,
        failed: 0,
        break: 0.60
    },
    12: {
        success: 0.40,
        failed: 0,
        break: 0.60
    },
    13: {
        success: 0.40,
        failed: 0,
        break: 0.60
    },
    14: {
        success: 0.40,
        failed: 0,
        break: 0.60
    },
    15: {
        success: 0.40,
        failed: 0,
        break: 0.60
    }
})

const zenyPerRefine = ref([
    15000,
    25000,
    35000,
    45000,
    55000,
    65000,
    75000,
    85000,
    95000,
    105000,
    125000,
    125000,
    125000,
    125000,
    125000
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

const isDisabledRefine = computed(() => {
    return props.zeny < zenyPerRefine.value[refineCount.value] || refineCount.value == 15
})

const isDisabledRepair = computed(() => {
    return props.zeny < props.equipment
})

const refine = () => {
    if(refineCount.value == 15){
        return
    }

    emit('update:zeny', props.zeny - zenyPerRefine.value[refineCount.value])
    emit('update:expense', zenyPerRefine.value[refineCount.value])

    refineResult()

    if(refineCount.value < 4){
        result.value = 'success'
        refineCount.value++
        return
    }

    if(isDisabledRefine.value){
        return
    }

    if (result.value == 'success') {
        refineCount.value++
        return
    }

    refineCount.value--

    if(result.value == 'broken'){
        isBroken.value = true
        return
    }
}

const repair = () => {
    emit('update:zeny', props.zeny - props.equipment)
    emit('update:expense', props.equipment)
    emit('update:eqRepair', 1)
    isBroken.value = false
    result.value = 'repaired'
}

const refineResult = () => {
    const rand = Math.random()
    let successRate = refineRating[refineCount.value + 1].success
    let failedRate = refineRating[refineCount.value + 1].failed
    let breakRate = refineRating[refineCount.value + 1].break

    console.log(`rand: ${rand}`)
    console.log(`successRate: ${successRate}`)
    console.log(`failedRate: ${failedRate}`)
    console.log(`breakRate: ${breakRate}`)

    if(rand < successRate){
        result.value = 'success'
        return
    }
    else if(rand < successRate + failedRate){
        result.value = 'failed'
        return
    }
    else if(rand < successRate + failedRate + breakRate){
        result.value = 'broken'
    }
}
</script>