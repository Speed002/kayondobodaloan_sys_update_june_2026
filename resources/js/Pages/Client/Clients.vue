<script setup>
import { Head, router  } from '@inertiajs/vue3'
import Default from '@/Layouts/Default.vue'
import ClientCard from './ClientCard.vue';
import _debounce from 'lodash.debounce';
import { ref, watch } from 'vue';
import Pagination from '../../Components/Pagination.vue';
defineOptions({ layout: Default })
const props = defineProps({
    clients:Object,
    query:Object
})
const searchQuery = ref(props.query.search ||'')
const handleSearchInput = _debounce((query) => {
    router.reload({
        data:{
            search: query
        },
        preserveScroll:true
    })
}, 500)
watch(searchQuery, (query) => {
    handleSearchInput(query)
})
</script>
<template>
    <div class="px-4 py-6 md:px-6 lg:px-8">
        <div class="max-w-7xl mx-auto space-y-6">

            <!-- Search -->
            <input
                type="search"
                id="search"
                class="w-full rounded-md border-gray-300 text-sm focus:ring focus:ring-indigo-200"
                v-model="searchQuery"
                placeholder="Search clients..."
            />

            <div v-if="clients" class="space-y-6">

            <!-- Clients Grid -->
            <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4">
                <ClientCard
                    v-for="client in clients.data"
                    :key="client.id"
                    :client="client"
                />
            </div>

                <Pagination :pagination="clients.meta"/>

            </div>

            <div v-else class="text-gray-500 text-sm">
                No clients yet, please create!
            </div>

        </div>
    </div>

    <Head title="Clients" />
</template>
