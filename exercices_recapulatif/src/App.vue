<template>
  <div class="container my-5">
    <h1 class="text-center mb-4 text-primary">📝 Gestion des Étudiants Potentiels</h1>
    
    <div class="row">
      <div class="col-lg-5 mb-4">
        <StudentForm @add-student="addStudent" />
      </div>

      <div class="col-lg-7">
        <StudentList :students="students" />
      </div>
    </div>
  </div>
</template>

<script>
import StudentForm from './components/StudentForm.vue'
import StudentList from './components/StudentList.vue'


export default {
  components: { StudentForm, StudentList },
  data() {
    return {
      // Pour la persistance (bonus), on pourrait initialiser ici avec localStorage
      students: [] 
    }
  },
  methods: {
    // Méthode appelée lorsque StudentForm émet 'add-student'
    addStudent(student) {
      // Ajout d'un ID pour identifier chaque étudiant (bonne pratique)
      this.students.push({
        id: Date.now(), // ID basé sur l'horodatage
        ...student
      })
    }
  }
}
</script>