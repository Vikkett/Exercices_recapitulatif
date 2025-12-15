<template>
    <!-- Vérifie s'il y a des étudiants à afficher -->
    <div v-if="students.length" class="mb-5">
      
      <!-- Titre avec nombre total d'étudiants -->
      <h3 class="text-center mb-4 text-primary">Liste des Étudiants ({{ students.length }})</h3>
  
      <!-- Conteneur de cartes des étudiants avec espacement -->
      <div class="row justify-content-start g-3">
  
        <!-- Boucle sur la liste des étudiants pour créer une carte par étudiant -->
        <div class="col-md-6 col-lg-6" v-for="student in students" :key="student.id">
          
          <!-- Carte individuelle pour chaque étudiant -->
          <div class="card h-100 shadow-sm border-success">
            <div class="card-body">
              
              <!-- Nom complet de l'étudiant -->
              <h5 class="card-title text-success mb-2">{{ student.firstName }} {{ student.lastName }}</h5>
              
              <!-- Ligne de séparation -->
              <hr class="my-2">
              
              <!-- Liste des informations de base -->
              <ul class="list-unstyled small">
                <!-- Sexe -->
                <li><strong>Sexe :</strong> {{ student.gender }}</li>
  
                <!-- Date de naissance et âge calculé -->
                <li>
                  <strong>Date de naissance :</strong> {{ formatDate(student.birthDate) }}
                  <span class="badge bg-secondary ms-2">{{ calculateAge(student.birthDate) }} ans</span>
                </li>
  
                <!-- Niveau de motivation affiché dans un badge -->
                <li><strong>Motivation :</strong> <span class="badge bg-info text-dark">{{ student.motivation }}</span></li>
  
                <!-- Emoji représentant le caractère / humeur -->
                <li><strong>Caractère :</strong> <span class="fs-4">{{ student.moodEmoji }}</span></li>
              </ul>
  
              <!-- Ligne de séparation -->
              <hr class="my-2">
  
              <!-- Section pour les centres d'intérêt -->
              <p class="fw-bold mb-1 small">Centres d’intérêt ({{ student.interests.length }}/3) :</p>
              <div>
                <!-- Message si aucun centre d'intérêt sélectionné -->
                <span v-if="student.interests.length === 0" class="badge bg-warning text-dark">Aucun sélectionné</span>
  
                <!-- Boucle sur les centres d'intérêt pour créer un badge pour chacun -->
                <span 
                  v-for="interest in student.interests" 
                  :key="interest" 
                  class="badge bg-primary me-1 mb-1"
                >
                  {{ interest }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  
    <!-- Message affiché si aucun étudiant n'est présent -->
    <div v-else class="alert alert-info text-center shadow-sm">
      <h4 class="alert-heading">Liste des Étudiants</h4>
      <p class="mb-0">Aucun étudiant n'a encore été inscrit. Utilisez le formulaire à gauche pour commencer.</p>
    </div>
  </template>
  
  <script>
  export default {
    // Déclaration des props reçues depuis le composant parent
    props: {
      students: {
        type: Array,    // La prop doit être un tableau
        required: true  // Obligatoire pour le composant
      }
    },
    methods: {
      // Fonction pour calculer l'âge à partir de la date de naissance
      calculateAge(birthDate) {
        const today = new Date();             // Date actuelle
        const birth = new Date(birthDate);    // Conversion de la date de naissance en objet Date
        let age = today.getFullYear() - birth.getFullYear(); // Différence en années
        const m = today.getMonth() - birth.getMonth();      // Calcul du mois
        // Ajustement si le mois/jour n'est pas encore passé cette année
        if (m < 0 || (m === 0 && today.getDate() < birth.getDate())) {
          age--;
        }
        return age; // Retourne l'âge calculé
      },
  
      // Fonction pour formater la date en format français lisible
      formatDate(dateString) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' }; // Options de formatage
        return new Date(dateString).toLocaleDateString('fr-FR', options);  // Conversion en chaîne locale
      }
    }
  }
  </script>
  