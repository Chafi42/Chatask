<template>
  <section>
    <div class="calendar-header">
      <button @click="previousMonth">Précédent</button>
      <h2>{{ meses[mesSelec] }} {{ annioSelec }}</h2>
      <button @click="nextMonth">Suivant</button>
    </div>

    <table class="calendrier">
      <thead>
        <tr>
          <th><abbr title="Lundi">Lundi</abbr></th>
          <th><abbr title="Mardi">Mardi</abbr></th>
          <th><abbr title="Mercredi">Mercredi</abbr></th>
          <th><abbr title="Jeudi">Jeudi</abbr></th>
          <th><abbr title="Vendredi">Vendredi</abbr></th>
          <th><abbr title="Samedi">Samedi</abbr></th>
          <th><abbr title="Dimanche">Dimanche</abbr></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(semana, sIndex) in bloquesCalendario" :key="sIndex">
          <td v-for="(dia, dIndex) in semana" :key="dIndex" @click="openTaskModal(sIndex, dIndex)">
            <span :class="{'current': isToday(dia.dia)}">
              {{ dia.dia || '' }}
            </span>
            <div v-if="dia.tasks.length > 0">
              <ul>
                <li v-for="(task, tIndex) in dia.tasks" :key="tIndex" class="task">
                  {{ task.text }}
                  <button @click="removeTask(sIndex, dIndex, tIndex)" class="text-red-500">Supprimer</button>
                </li>
              </ul>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modale pour ajouter une tâche -->
    <div v-if="isModalOpen" class="modal">
      <div class="modal-content">
        <h3>Ajouter une tâche</h3>
        <input type="text" v-model="newTaskText" placeholder="Entrer une nouvelle tâche" />
        <div class="modal-actions">
          <button @click="closeTaskModal">Annuler</button>
          <button @click="addTask">Ajouter</button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      bloquesCalendario: [], // Calendrier en semaines
      annioSelec: new Date().getFullYear(), // Année sélectionnée
      mesSelec: new Date().getMonth(), // Mois sélectionné
      currentDay: new Date().getDate(), // Jour actuel
      newTaskText: '', // Texte de la nouvelle tâche
      isModalOpen: false, // État de la modale
      selectedDay: { semanaIndex: null, diaIndex: null }, // Jour sélectionné
      meses: ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"]
    };
  },
  created() {
    this.genererCalendrier();
  },
  methods: {
    // Génération du calendrier pour le mois sélectionné
    genererCalendrier() {
      const primerDiaMes = new Date(this.annioSelec, this.mesSelec, 1).getDay(); // Premier jour du mois
      const diasMes = new Date(this.annioSelec, this.mesSelec + 1, 0).getDate(); // Nombre de jours dans le mois

      let dia = 1;
      const bloques = [];

      // Remplissage du calendrier par semaine
      for (let semana = 0; semana < 6; semana++) {
        const fila = [];
        for (let diaSemana = 0; diaSemana < 7; diaSemana++) {
          if (semana === 0 && diaSemana < (primerDiaMes || 7) - 1) {
            fila.push({ dia: '', tasks: [] }); // Jours avant le début du mois
          } else if (dia > diasMes) {
            fila.push({ dia: '', tasks: [] }); // Jours après la fin du mois
          } else {
            fila.push({ dia: dia, tasks: [] }); // Jours du mois
            dia++;
          }
        }
        bloques.push(fila);
      }
      this.bloquesCalendario = bloques;
    },
    // Aller au mois suivant
    nextMonth() {
      this.mesSelec++;
      if (this.mesSelec > 11) {
        this.mesSelec = 0;
        this.annioSelec++;
      }
      this.genererCalendrier();
    },
    // Aller au mois précédent
    previousMonth() {
      this.mesSelec--;
      if (this.mesSelec < 0) {
        this.mesSelec = 11;
        this.annioSelec--;
      }
      this.genererCalendrier();
    },
    // Ouvrir la modale d'ajout de tâche
    openTaskModal(semanaIndex, diaIndex) {
      this.selectedDay = { semanaIndex, diaIndex };
      this.newTaskText = '';
      this.isModalOpen = true;
    },
    // Fermer la modale
    closeTaskModal() {
      this.isModalOpen = false;
    },
    // Ajouter une tâche au jour sélectionné
    addTask() {
      const { semanaIndex, diaIndex } = this.selectedDay;
      if (this.newTaskText.trim()) {
        this.bloquesCalendario[semanaIndex][diaIndex].tasks.push({ text: this.newTaskText });
        this.newTaskText = '';
        this.closeTaskModal();
      }
    },
    // Supprimer une tâche d'un jour donné
    removeTask(semanaIndex, diaIndex, taskIndex) {
      this.bloquesCalendario[semanaIndex][diaIndex].tasks.splice(taskIndex, 1);
    },
    // Vérifier si le jour est aujourd'hui
    isToday(day) {
      const today = new Date();
      return day === today.getDate() && this.mesSelec === today.getMonth() && this.annioSelec === today.getFullYear();
    }
  }
};
</script>

<style>
/* Styles pour le calendrier */
.calendrier {
  background-color: #F6F6F6;
  padding: 2em;
  width: 100%;
}

thead {
  color: #D35A33;
}

.calendrier caption {
  background-color: #D35A32;
  color: #F6F6F6;
  text-align: left;
  padding: 30px 20px;
  font-size: 200%;
  text-transform: uppercase;
  line-height: 0.9;
}

.calendrier td, .calendrier th {
  text-align: center;
  padding: .8em;
}

.current {
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  background-color: #D35A32;
  color: #f6f6f6;
  border-radius: 100%;
}

.task {
  margin-top: 5px;
  font-size: 0.8em;
}

/* Styles pour la modale */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  width: 300px;
  text-align: center;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

button {
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>