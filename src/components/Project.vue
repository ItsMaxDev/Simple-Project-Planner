<script setup>
import { ref } from 'vue';

const showDescription = ref(false);

const props = defineProps({
    project: {
        type: Object,
        required: true,
        validator: function (object) {
            return 'id' in object && 'name' in object && 'description' in object && 'completed' in object;
        }
    }
})

const emits = defineEmits(['delete', 'edit', 'complete']);

function toggleDescription() {
    showDescription.value = !showDescription.value
}

</script>

<template>
    <div v-if="project">
        <div class="p-10 bg-base-300 rounded-lg text-bl shadow-lg border-l-8" :class="[project.completed ? 'border-green-500' : 'border-red-500']">
            <div class="flex justify-between items-center">
                <span @click="toggleDescription" v-if="!showDescription" class="material-icons">chevron_right</span>
                <span @click="toggleDescription" v-if="showDescription" class="material-icons">expand_more</span>
                <h2>{{ project.name }}</h2>
                <div class="flex items-center space-x-1">
                    <span @click="$emit('delete', project)" class="material-icons hover:text-red-500">delete</span>
                    <span @click="$emit('edit', project)" class="material-icons hover:text-orange-500">edit</span>
                    <span @click="$emit('complete', project)" class="material-icons hover:text-green-500" :class="{ 'text-green-500': project.completed }">done</span>
                </div>
            </div>
            <div v-if="showDescription" class="mt-5">
                <p>{{ project.description }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
.material-icons {
    @apply hover:bg-white;
    @apply hover:bg-opacity-10;
    @apply hover:rounded-full;
    @apply select-none;
    @apply cursor-pointer;
    @apply p-2;
}
</style>