<template>
    <div>
        <Nav />
        <div class="flex justify-center space-x-4 my-32">
            <button v-if="currentComponent !== TaskComponent" @click="currentComponent = TaskComponent" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
                Tâches
            </button>
            <button v-if="currentComponent !== AgendaComponent" @click="currentComponent = AgendaComponent" :class="invisible" class="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600">
                Agenda
            </button>
        </div>
        <component :is="currentComponent" />
        <Footer />
    </div>
</template>

<script setup>
import { ref } from 'vue';
import Nav from './components/Nav.vue';
import Footer from './components/Footer.vue';
import TaskComponent from './components/Task.vue';
import AgendaComponent from './components/Agenda.vue';

// Initialisation du composant courant
const currentComponent = ref(TaskComponent);
</script>