<script>
export default {
    name: 'LessonsForm'
}
</script>

<script setup>
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import FormSection from "@/Components/FormSection.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import CollectionSelector from "@/Components/Common/CollectionSelector.vue";
import {ref} from "vue";

defineProps({
    form: {
        type: Object,
        required: true
    },
    updating: {
        type: Boolean,
        required: false,
        default: false
    },
    categories: {
        type: Object,
        required: true
    },
    levels: {
        type: Object,
        required: true
    }
})

const categoriesLesson = ref([])

const onCategories = _categories => {
    categoriesLesson.value = _categories
    console.log(categories)
}

defineEmits(['submit'])
</script>

<template>
    <FormSection @submitted="$emit('submit')">
        <template #title>
            {{ updating ? 'Update Lesson' : 'Create New Lesson' }}
        </template>

        <template #form>
            <div class="space-y-4 col-span-6 sm:col-span-6">
                <div>
                    <InputLabel for="name" value="Name"/>
                    <TextInput id="name" v-model="form.name" type="text" autocomplete="name" class="mt-1 block w-full"/>
                    <InputError :message="$page.props.errors.name" class="mt-2"/>
                </div>
                <div>
                    <InputLabel for="description" value="Description"/>
                    <TextInput id="description" v-model="form.description" type="text" autocomplete="description"
                               class="mt-1 block w-full"/>
                    <InputError :message="$page.props.errors.description" class="mt-2"/>
                </div>
                <div>
                    <InputLabel for="content_uri" value="Conten uri"/>
                    <TextInput id="content_uri" v-model="form.content_uri" type="text" autocomplete="content_uri"
                               class="mt-1 block w-full"/>
                    <InputError :message="$page.props.errors.content_uri" class="mt-2"/>
                </div>
                <div>
                    <InputLabel value="PDF"/>
                    <SecondaryButton type="button" class="hover:border-indigo-500 hover:text-indigo-500">UPLOAD PDF
                    </SecondaryButton>
                    <InputError :message="$page.props.errors.pdf_uri" class="mt-2"/>
                </div>
                <div class="flex ">
                    <div class="w-1/2 mr-1">
                        <InputLabel for="level_id" value="Level"/>
                        <select
                            class="border-gray-300 focus:border-indigo-500 focus:ring-indigo-500 rounded-md shadow-sm w-full">
                            <option v-for="level in levels" :value="level.id">{{ level.name }}</option>
                        </select>
                        <InputError :message="$page.props.errors.level_id" class="mt-2"/>
                    </div>
                    <div class="w-1/2 ml-1">
                        <InputLabel for="categories" value="Categories"/>
                        <CollectionSelector :collection="categories" @onCategories="onCategories"/>
                    </div>
                </div>
            </div>
        </template>

        <template #actions>
            <PrimaryButton>
                {{ updating ? 'Update' : 'Create' }}
            </PrimaryButton>
        </template>

    </FormSection>
</template>
