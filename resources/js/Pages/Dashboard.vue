<script setup>
import { Head, Link } from '@inertiajs/vue3'
import Default from '@/Layouts/Default.vue'
import { PlusIcon,  } from '@heroicons/vue/24/outline';
defineOptions({ layout: Default })
defineProps({
    clients:Object,
    latest_client:Object
})
</script>
<template>
    <div class="p-8 font-sans">
        <div class="max-w-5xl mx-auto space-y-5">
            <h3 class="text-sm">Dashboard</h3>
            <div class="flex-grow rounded-md">
                <div class="flex items-center justify-between">
                    <div v-if="clients" class="flex items-center justify-between md:justify-start sm:justify-start space-x-3 rounded-md text-sm">
                        <Link :href="route('clients')" class="portal rounded-md flex items-center ">
                            <span class="text-sky-500">{{clients}}&nbsp;</span>
                            clients 
                        </Link>
                    </div>
                    <Link :href="route('create.client')" class="bg-sky-700 text-sm portal tracking-tighter rounded-md flex items-center justify-between hover:bg-transparent-dark">
                        <PlusIcon class="w-4 h-4"/>
                        &nbsp;Add client
                    </Link>
                </div>
            </div>
            <div v-if="latest_client" class="flex-grow portal-mini rounded-md space-y-1">
                <div class="flex flex-col space-y-2">
                    <span class="text-sm">Recently added client:</span>
                    <Link :href="route('client.show', latest_client.id)" class="text-xs text-sky-500">
                        <span class="text-gray-400">></span> {{latest_client.name}}, {{latest_client.nin}}
                    </Link>
                </div>
            </div>
        </div>
    </div>
    <Head title="Dashboard" />
</template>
<style scoped>
.portal{
    background-color: rgba(0, 0, 0, 0.3) !important;
    padding:10px;
}
.portal-mini{
    background-color: rgba(0, 0, 0, 0.3) !important;
    padding:10px;
}
</style>
