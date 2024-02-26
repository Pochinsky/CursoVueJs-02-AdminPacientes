<script setup>
import { ref, reactive, watch, onMounted } from 'vue'
import { uid } from 'uid'
import Header from './components/Header.vue'
import Form from './components/Form.vue'
import Patient from './components/Patient.vue'
import Alert from './components/Alert.vue'

// states
const patients = ref([])
const patient = reactive({
  id: null,
  animal: '',
  cuidador: '',
  email: '',
  alta: '',
  sintomas: ''
})
const alertDelete = reactive({
  type: '',
  message: '',
})

// init states
onMounted(() => {
  const patientsStorage = localStorage.getItem('patients')
  if (patientsStorage) {
    patients.value = JSON.parse(patientsStorage)
  }
})

// functions
const saveLocalStorage = () => {
  localStorage.setItem('patients', JSON.stringify(patients.value))
}

const savePatient = () => {
  if (patient.id) {
    const { id } = patient
    const index = patients.value.findIndex(statePatient => statePatient.id === id)
    patients.value[index] = { ...patient }
  }
  else patients.value.push({ ...patient, id: uid() })
  Object.assign(patient, {
    id: null,
    animal: '',
    cuidador: '',
    email: '',
    alta: '',
    sintomas: ''
  })
}

const updatePatient = (id) => {
  const editPatient = patients.value.filter(patient => patient.id === id)[0]
  Object.assign(patient, editPatient)
}

const deletePatient = (id) => {
  patients.value = patients.value.filter(patient => patient.id !== id)
  alertDelete.type = 'exito'
  alertDelete.message = 'Paciente eliminado correctamente'
  setTimeout(() => {
    Object.assign(alertDelete, {
      type: '',
      message: '',
    })
  }, 3000);
}

// listener
watch(patients, () => { saveLocalStorage() }, { deep: true })
</script>

<template>
  <div class="container mx-auto pt-4">
    <Header />
    <div class="pt-8 md:flex">
      <Form v-model:animal="patient.animal" v-model:cuidador="patient.cuidador" v-model:email="patient.email"
        v-model:alta="patient.alta" v-model:sintomas="patient.sintomas" :id="patient.id" @save-patient="savePatient" />
      <div class="md:w-1/2">
        <h3 class="font-bold text-2xl text-center">Administra tus Pacientes</h3>
        <p class="mt-4 text-lg text-center mb-8">
          Información de <span class="text-indigo-600 font-bold">Pacientes</span>
        </p>
        <div v-if="patients.length > 0" class="md:h-4/5 overflow-y-scroll">
          <Alert v-if="alertDelete.message" :alert="alertDelete" />
          <Patient v-for="patient in patients" :patient="patient" @update-patient="updatePatient"
            @delete-patient="deletePatient" />
        </div>
        <p v-else class="mt-4 text-lg text-center">No tienes pacientes agregados</p>
      </div>
    </div>
  </div>
</template>
