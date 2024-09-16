<template>  
  <div class="container mx-auto mb-32 p-4">
    <!-- Barre de navigation -->
    <div class="flex flex-col md:flex-row justify-between items-center border-b border-gray-300 pb-4">
      <a id="Titulo_Calendario" class="text-xl font-bold text-center md:text-left">
        Calendrier {{ meses[mesSelec] }} {{ annioSelec }}
      </a>
      <div class="flex space-x-4 mt-4 md:mt-0">
        <!-- Bouton gauche -->
        <button class="focus:outline-none" id="b_ant" @click="anteriorMes">
          <img class="transform rotate-180 w-6 h-6" src="https://img.icons8.com/ios-glyphs/30/000000/arrow.png"/>
        </button>
        <!-- Bouton droit -->
        <button class="focus:outline-none" id="b_der" @click="siguienteMes">
          <img class="w-6 h-6" src="https://img.icons8.com/ios-glyphs/30/000000/arrow.png"/>
        </button>
      </div>
    </div>

    <!-- Grille du calendrier -->
    <div class="grid grid-cols-7 gap-2 mt-4 text-center">
      <!-- Jours de la semaine -->
      <div class="font-semibold" v-for="diaSemana in diasSemana" :key="diaSemana">{{ diaSemana }}</div>

      <!-- Blocs du calendrier -->
      <div v-for="(bloque, index) in bloques" :key="index" :style="{ backgroundColor: bloque.dia ? '#DE6614' : '' }" class="h-20 rounded-lg">
        <div v-if="bloque.dia" class="flex flex-col justify-between p-1 text-white font-bold">
          <span>{{ bloque.dia }}</span>
          <div v-for="(task, taskIndex) in bloque.tasks" :key="taskIndex" class="text-sm">
            {{ task.text }}
            <button class="ml-2 text-red-500" @click="removeTask(index, taskIndex)">
              Supprimer
            </button>
          </div>
          <input class="w-full p-1 mt-1 text-black" v-model="newTask" @keyup.enter="addTask(index)" placeholder="Ajouter une tâche">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    name: 'Agenda'
    return {
      bloques: Array.from({ length: 42 }, () => ({ dia: '', tasks: [] })),
      annioSelec: new Date().getFullYear(),
      mesSelec: new Date().getMonth(),
      diaHoy: new Date().getDate(),
      diasSemana: ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"],
      meses: [
        "Janvier", "Février", "Mars", "Avril", "Mai", "Juin", 
        "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"
      ],
      newTask: '', // Correctement défini ici
    };
  },
  created() {
    this.llenarCalendario(this.annioSelec, this.mesSelec);
  },
  methods: {
    llenarCalendario(annio, mes) {
      const primerDiaMes = new Date(annio, mes, 1).getDay();
      const diasMes = new Date(annio, mes + 1, 0).getDate();

      let dia = 1;
      for (let i = primerDiaMes; i < diasMes + primerDiaMes; i++) {
        this.bloques[i].dia = dia;
        dia++;
      }
    },
    siguienteMes() {
      this.mesSelec++;
      if (this.mesSelec > 11) {
        this.mesSelec = 0;
        this.annioSelec++;
      }
      this.resetCalendario();
    },
    anteriorMes() {
      this.mesSelec--;
      if (this.mesSelec < 0) {
        this.mesSelec = 11;
        this.annioSelec--;
      }
      this.resetCalendario();
    },
    addTask(index) {
      if (this.newTask.trim()) {
        this.bloques[index].tasks.push({ text: this.newTask });
        this.newTask = ''; // Vider le champ après ajout
      }
    },
    removeTask(bloqueIndex, taskIndex) {
      this.bloques[bloqueIndex].tasks.splice(taskIndex, 1);
    },
    resetCalendario() {
      this.bloques = Array.from({ length: 42 }, () => ({ dia: '', tasks: [] }));
      this.llenarCalendario(this.annioSelec, this.mesSelec);
    }
  }

};
</script>
