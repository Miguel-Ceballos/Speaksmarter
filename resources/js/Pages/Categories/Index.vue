<script>
export default {
    name: 'CategoriesIndex'
}
</script>

<script setup>
import AppLayout from "@/Layouts/AppLayout.vue";
import {Link} from "@inertiajs/vue3";
import {Inertia} from "@inertiajs/inertia";

defineProps({
    categories: {
        type: Object,
        required: true
    }
})

const deleteCategory = id => {
    if (confirm('Are you sure?')) {
        Inertia.delete(route('categories.destroy', id))
    }
}

</script>

<template>
    <AppLayout>
        <template #header>
            <h1 class="font-bold text-xl text-gray-800 leading-tight">Categories</h1>
        </template>

        <div class="py-12 ">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="p-6 bg-white border-b border-gray-200">
                    <div class="flex justify-between">
                        <!-- Todo: Create category -->
                        <Link :href="route('categories.create')"
                              class="bg-indigo-500 font-bold text-white p-2 rounded hover:bg-indigo-600 transition"
                              v-if="$page.props.user.permissions.includes('create categories')"
                        >
                            CREATE CATEGORY
                        </Link>
                    </div>

                    <div class="mt-4">
                        <ul role="list" class="divide-y divide-gray-200">
                            <li class="flex justify-between gap-x-6 py-5" v-for="category in categories.data"
                                :key="category.id">
                                <div class="flex min-w-0 gap-x-4">
                                    <div class="min-w-0 flex-auto">
                                        <p class="text-md font-semibold leading-6 text-gray-800">{{ category.name }}</p>
                                    </div>
                                </div>
                                <div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
                                    <div class="flex gap-4">
                                        <Link :href="route('categories.edit', category.id)"
                                              class="text-md leading-6 text-blue-600"
                                              v-if="$page.props.user.permissions.includes('update categories')"
                                        >
                                            Edit
                                        </Link>
                                        <Link
                                            @click="deleteCategory(category.id)"
                                            class="text-md leading-6 text-red-600"
                                            v-if="$page.props.user.permissions.includes('delete categories')"
                                        >
                                            Delete
                                        </Link>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                    <div class="flex justify-between mt-4">
                        <Link v-if="categories.current_page > 1" :href="categories.prev_page_url"
                            class="border-2 border-indigo-500 font-bold text-indigo-500 p-2 rounded hover:border-indigo-700 hover:text-indigo-700 transition"
                        >
                            PREV
                        </Link>
                        <div v-else></div>
                        <Link v-if="categories.current_page < categories.last_page" :href="categories.next_page_url"
                            class="border-2 border-indigo-500 font-bold text-indigo-500 p-2 rounded hover:border-indigo-700 hover:text-indigo-700 transition"
                        >
                            NEXT
                        </Link>
                    </div>
                </div>
            </div>
        </div>

    </AppLayout>
</template>
