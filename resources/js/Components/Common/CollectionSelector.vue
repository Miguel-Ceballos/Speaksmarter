<script setup>
import {ref} from "vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";

defineProps({
    collection: {
        type: Array,
        required: true
    }
})

const emit = defineEmits(['onCategories'])

const currentSelection = ref(1)

const selection = ref([])

const handleAddToSelection = () => {
    const isExist = selection.value.findIndex(item => item.id === currentSelection.value.id)

    if (isExist < 0) {
        selection.value.push(currentSelection.value)
        emit("onCategories", selection.value)
    }

}

const handleRemoveSelection = index => {
    selection.value = selection.value.filter(item => item.id !== index)
}

</script>

<template>
    <div class="w-full">
        <div class="flex">
            <select v-model="currentSelection"
                    class="border-gray-300 focus:border-indigo-500 focus:ring-indigo-500 rounded-md shadow-sm w-full">
                <option v-for="(item, index) in collection" :key="index" :value="item">{{ item?.name }}</option>
            </select>

            <SecondaryButton @click="handleAddToSelection">Add</SecondaryButton>
        </div>
        <div>
            <ul>
                <li class="px-3 py-2 border border-gray-300 rounded-md shadow-sm justify-between flex hover:bg-gray-100"
                    v-for="(item, index) in selection" :key="index"
                >
                    {{ item.name }}
                    <button
                        class="bg-gray-200 text-gray-700 text-sm w-5 h-5 rounded-full hover:bg-gray-300 hover:text-gray-900"
                        type="button"
                        @click="handleRemoveSelection(item.id)"
                    >x
                    </button>
                </li>
            </ul>
        </div>
    </div>

</template>
