<script setup>
import { 
    UserCircleIcon, 
    BanknotesIcon, 
    UsersIcon, 
    TruckIcon, 
    DocumentArrowDownIcon, 
    InformationCircleIcon, 
    ChevronLeftIcon, 
    ChevronRightIcon, 
    TrashIcon
} from '@heroicons/vue/24/outline';

import { Link } from '@inertiajs/vue3';
import { ref, onMounted, nextTick } from 'vue';

defineProps({
    info: Number,
    client: Object,
    motor: Object
});

const emit = defineEmits(['update:info']);

const scrollContainer = ref(null);
const showLeftArrow = ref(false);
const showRightArrow = ref(false);

const checkScroll = () => {
    if (!scrollContainer.value) return;
    
    const el = scrollContainer.value;
    showLeftArrow.value = el.scrollLeft > 10;
    showRightArrow.value = el.scrollLeft < el.scrollWidth - el.clientWidth - 10;
};

const scrollLeft = () => {
    if (scrollContainer.value) {
        scrollContainer.value.scrollBy({
            left: -180,   // adjust this value for how much to scroll per click
            behavior: 'smooth'
        });
    }
};

const scrollRight = () => {
    if (scrollContainer.value) {
        scrollContainer.value.scrollBy({
            left: 180,
            behavior: 'smooth'
        });
    }
};

onMounted(() => {
    nextTick(() => {
        checkScroll();
        if (scrollContainer.value) {
            scrollContainer.value.addEventListener('scroll', checkScroll);
            // Also check after resize
            window.addEventListener('resize', checkScroll);
        }
    });
});
</script>

<template>
    <div class="md:space-y-6 sm:space-y-3 space-y-2 w-full">
        <h4 class="text-sm text-gray-300 md:text-xs sm:text-xs font-semibold flex items-center gap-1">
           <UserCircleIcon class="h-6 w-6" /> 
           {{ client.name }}
        </h4>

        <!-- Scrollable area with arrows -->
        <div class="relative w-full">
            
            <!-- Left Arrow -->
            <button
                v-if="showLeftArrow"
                @click="scrollLeft"
                class="md:hidden absolute left-0 top-4 -translate-y-1/2 z-10 text-white p-1 rounded-md transition-all"
            >
                <ChevronLeftIcon class="h-5 w-5" />
            </button>

            <!-- Scroll Container -->
            <div 
                ref="scrollContainer"
                class="overflow-x-auto pb-4 md:pb-0 scrollbar-hide scroll-smooth"
                @scroll="checkScroll"
            >
                <div class="flex md:flex-col flex-nowrap gap-2 md:gap-3 min-w-max md:min-w-0">
                    
                    <button
                        @click="emit('update:info', 2)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 2}"
                    >
                        <TruckIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">Motor</span>
                        <span class="hidden md:inline">Motor information</span>
                    </button>

                    <button
                        @click="emit('update:info', 3)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 3}"
                    >
                        <UsersIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">Referees</span>
                        <span class="hidden md:inline">Referee information</span>
                    </button>

                    <button
                        @click="emit('update:info', 4)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 4}"
                    >
                        <BanknotesIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">Loan</span>
                        <span class="hidden md:inline">Loan information</span>
                    </button>

                    <button
                        @click="emit('update:info', 5)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 5}"
                    >
                        <DocumentArrowDownIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">PDFs</span>
                        <span class="hidden md:inline">PDF Downloads</span>
                    </button>

                    <button
                        @click="emit('update:info', 6)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 6}"
                    >
                        <InformationCircleIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">Files</span>
                        <span class="hidden md:inline">Client Files</span>
                    </button>

                    <button
                        @click="emit('update:info', 1)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 1}"
                    >
                        <UserCircleIcon class="h-4 w-4 mr-1.5" />
                        <span class="md:hidden text-xs">Personal</span>
                        <span class="hidden md:inline">Personal info</span>
                    </button>

                    <Link
                        :href="route('delete.client.index', client.id)"
                        class="flex-shrink-0 hover:bg-transparent-dark px-4 py-2.5 text-sm tracking-tighter rounded-md flex items-center whitespace-nowrap transition-colors snap-start"
                        :class="{'bg-transparent-dark': info === 1}"
                    >
                        <span class="md:hidden text-xs">
                            <TrashIcon class="h-4 w-4 text-rose-600" />
                        </span>
                        <span class="hidden md:inline">
                            <TrashIcon class="h-4 w-4 text-rose-600" />
                        </span>
                    </Link>

                </div>
            </div>

            <!-- Right Arrow -->
            <button
                v-if="showRightArrow"
                @click="scrollRight"
                class="md:hidden absolute right-0 top-4 -translate-y-1/2 z-10 text-white p-2 rounded-l-md transition-all"
            >
                <ChevronRightIcon class="h-5 w-5" />
            </button>
        </div>
    </div>
</template>

<style scoped>
.scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
}

.scrollbar-hide::-webkit-scrollbar {
    display: none;
}
</style>