<template>
    <!-- Carte principale contenant le formulaire -->
    <div class="card card-body shadow-sm">
      
      <!-- Titre centré du formulaire -->
      <h3 class="text-center mb-3 text-success">Ajouter un Candidat</h3>
  
      <!-- Formulaire avec gestion de la soumission via la méthode submitStudent -->
      <form @submit.prevent="submitStudent">
  
        <!-- Ligne contenant Nom et Prénom -->
        <div class="row mb-3">
          <!-- Colonne pour le Nom -->
          <div class="col-md-6">
            <label class="form-label">Nom :</label>
            <!-- Champ texte pour le Nom avec liaison bidirectionnelle et validation obligatoire -->
            <input v-model.trim="lastName" class="form-control" required />
          </div>
  
          <!-- Colonne pour le Prénom -->
          <div class="col-md-6">
            <label class="form-label">Prénom :</label>
            <!-- Champ texte pour le Prénom avec liaison bidirectionnelle et validation obligatoire -->
            <input v-model.trim="firstName" class="form-control" required />
          </div>
        </div>
  
        <!-- Champ pour la date de naissance -->
        <div class="mb-3">
          <label class="form-label">Date de naissance :</label>
          <!-- Input type date lié à birthDate, obligatoire -->
          <input type="date" v-model="birthDate" class="form-control" required />
        </div>
  
        <!-- Section pour le choix du sexe -->
        <div class="mb-3">
          <label class="form-label d-block">Sexe :</label>
          <!-- Option Masculin -->
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" value="Masculin" v-model="gender" id="genderM"/> 
            <label class="form-check-label" for="genderM">Masculin</label>
          </div>
  
          <!-- Option Féminin -->
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" value="Féminin" v-model="gender" id="genderF"/> 
            <label class="form-check-label" for="genderF">Féminin</label>
          </div>
  
          <!-- Option Autre / Non spécifié -->
          <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" value="Autre / Non spécifié" v-model="gender" id="genderO"/> 
            <label class="form-check-label" for="genderO">Autre / Non spécifié</label>
          </div>
        </div>
  
        <!-- Sélecteur du niveau de motivation -->
        <div class="mb-3">
          <label class="form-label">Motivation à être là :</label>
          <select class="form-select" v-model="motivationLevel">
            <option value="Très motivé">Très motivé</option>
            <option value="Motivé">Motivé</option>
            <option value="Neutre">Neutre</option>
            <option value="Peu motivé">Peu motivé</option>
          </select>
        </div>
  
        <!-- Slider pour l’humeur / caractère -->
        <div class="mb-3">
          <label class="form-label d-block">Caractère / Humeur :</label>
          <input type="range" min="10" max="100" step="10" v-model.number="moodScore" class="form-range" />
          <!-- Affichage de l’emoji correspondant à l’humeur et du score -->
          <div class="text-center fs-3">
            {{ moodEmoji }} (Score: {{ moodScore }})
          </div>
        </div>
  
        <!-- Section pour les centres d’intérêt -->
        <div class="mb-3 p-3 border rounded" :class="{'border-danger': interests.length > 3}">
          <label class="form-label d-block text-decoration-underline">Centres d’intérêt (Max 3) :</label>
          <div class="d-flex flex-wrap gap-3">
            <!-- Boucle sur la liste d’intérêts pour créer les checkboxes -->
            <div class="form-check form-check-inline" v-for="interest in interestsList" :key="interest">
              <input
                class="form-check-input"
                type="checkbox"
                :value="interest"
                v-model="interests"
                :id="'int-' + interest"
                :disabled="interests.length >= 3 && !interests.includes(interest)"
              />
              <label class="form-check-label" :for="'int-' + interest">{{ interest }}</label>
            </div>
          </div>
          <!-- Message d’erreur si plus de 3 centres d’intérêt sont sélectionnés -->
          <small v-if="interests.length > 3" class="text-danger">Veuillez choisir 3 centres d'intérêt au maximum.</small>
        </div>
  
        <!-- Bouton de soumission désactivé si conditions non respectées -->
        <button class="btn btn-primary w-100" :disabled="interests.length > 3 || !gender || !firstName">
          Soumettre le candidat
        </button>
  
      </form>
    </div>
  </template>
  
  <script>
    // Fonction utilitaire pour obtenir un emoji en fonction du score d’humeur
    function getEmojiByMood(mood) {
      const emojis = ["😢", "😁", "😊", "😒", "😃", "😐", "😖", "😤", "😟", "😆"]
      // Calcul de l’index à partir de moodScore
      let index = Math.floor(mood / 10) - 1
      if (index < 0) index = 0
      if (index >= emojis.length) index = emojis.length - 1
      return emojis[index]
    }
  
    export default {
      // Définition des données réactives du composant
      data() {
        return {
          firstName: '',              // Prénom du candidat
          lastName: '',               // Nom du candidat
          gender: 'Autre / Non spécifié', // Sexe, valeur par défaut
          interests: [],              // Centres d’intérêt sélectionnés
          birthDate: '',              // Date de naissance
          moodScore: 50,              // Score de l’humeur, valeur par défaut
          motivationLevel: 'Très motivé', // Niveau de motivation par défaut
          interestsList: [            // Liste des centres d’intérêt possibles
            'Sport',
            'Environnement',
            'Science',
            'Technologie',
            'Actualité'
          ]
        }
      },
  
      // Propriétés calculées
      computed: {
        // Retourne l’emoji correspondant au score d’humeur
        moodEmoji() {
          return getEmojiByMood(this.moodScore)
        }
      },
  
      // Méthodes du composant
      methods: {
        submitStudent() {
          // Vérifie que le nombre de centres d’intérêt ne dépasse pas 3
          if (this.interests.length > 3) {
            alert("Veuillez choisir un maximum de 3 centres d'intérêt.");
            return;
          }
  
          // Émet l’événement 'add-student' avec les données du formulaire
          this.$emit('add-student', {
            firstName: this.firstName,
            lastName: this.lastName,
            gender: this.gender,
            interests: this.interests,
            birthDate: this.birthDate,
            motivation: this.motivationLevel,
            moodEmoji: this.moodEmoji
          })
  
          // Réinitialisation du formulaire
          this.firstName = ''
          this.lastName = ''
          this.gender = 'Autre / Non spécifié'
          this.interests = []
          this.birthDate = ''
          this.moodScore = 50
          this.motivationLevel = 'Très motivé'
        }
      }
    }
  </script>
  