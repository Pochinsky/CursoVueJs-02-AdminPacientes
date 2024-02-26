<script setup>
import { reactive, computed } from 'vue'
import Alert from './Alert.vue'

// props
const props = defineProps({
	animal: {
		type: String,
		required: true
	},
	cuidador: {
		type: String,
		required: true
	},
	email: {
		type: String,
		required: true
	},
	alta: {
		type: String,
		required: true
	},
	sintomas: {
		type: String,
		required: true
	},
	id: {
		type: [String, null],
		required: true,
	}
})

// states
const alert = reactive({
	type: '',
	message: '',
})

// emits
const emit = defineEmits(['update:animal', 'update:cuidador', 'update:email', 'update:alta', 'update:sintomas', 'save-patient'])

// functions
const validate = () => {
	if (Object.values(props).includes('')) {
		alert.type = 'error'
		alert.message = 'Todos los campos son obligatorios'
		setTimeout(() => {
			Object.assign(alert, {
				type: '',
				message: '',
			})
		}, 3000);
		return
	}
	emit('save-patient')
	alert.type = 'exito'
	alert.message = 'Paciente ' + (props.id ? 'actualizado' : 'registrado') + ' correctamente'
	setTimeout(() => {
		Object.assign(alert, {
			type: '',
			message: '',
		})
	}, 3000);
}

const editing = computed(() => {
	return props.id
})
</script>

<template>
	<div class="md:w-1/2 mx-8">
		<h2 class="font-bold text-2xl text-center">Registro de Pacientes</h2>
		<p class="text-lg mt-4 text-center mb-8">
			Agrega pacientes y <span class="text-indigo-600 font-bold">adminístralos</span>
		</p>
		<Alert v-if="alert.message" :alert="alert" />
		<form class="bg-white shadow-md rounded-lg p-8 mb-4" @submit.prevent="validate">
			<div class="mb-5">
				<label for="animal" class="block text-gray-700 uppercase font-bold">Nombre Animal</label>
				<input
					class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md hover:border-indigo-400 focus:border-indigo-600 focus:outline-none"
					id="animal" type="text" placeholder="Nombre del animal" :value="animal"
					@input="$emit('update:animal', $event.target.value)">
			</div>
			<div class="mb-5">
				<label for="cuidador" class="block text-gray-700 uppercase font-bold">Nombre Cuidador/a</label>
				<input
					class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md hover:border-indigo-400 focus:border-indigo-600 focus:outline-none"
					id="cuidador" type="text" placeholder="Nombre del cuidador/a" :value="cuidador"
					@input="$emit('update:cuidador', $event.target.value)">
			</div>
			<div class="mb-5">
				<label for="email" class="block text-gray-700 uppercase font-bold">Email Cuidador/a</label>
				<input
					class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md hover:border-indigo-400 focus:border-indigo-600 focus:outline-none"
					id="email" type="email" placeholder="cuidador@correo.com" :value="email"
					@input="$emit('update:email', $event.target.value)">
			</div>
			<div class="mb-5">
				<label for="alta" class="block text-gray-700 uppercase font-bold">Fecha Alta</label>
				<input
					class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md hover:border-indigo-400 focus:border-indigo-600 focus:outline-none"
					id="alta" type="date" :value="alta" @input="$emit('update:alta', $event.target.value)">
			</div>
			<div class="mb-5">
				<label for="sintomas" class="block text-gray-700 uppercase font-bold">Síntomas</label>
				<textarea
					class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md hover:border-indigo-400 focus:border-indigo-600 focus:outline-none"
					name="sintomas" id="sintomas" cols="30" rows="3" placeholder="Descripción de los síntomas"
					:value="sintomas" @input="$emit('update:sintomas', $event.target.value)"></textarea>
			</div>
			<input
				class="bg-indigo-600 w-full p-3 text-white uppercase font-bold rounded-md hover:bg-indigo-700 cursor-pointer transition-colors"
				type="submit" :value="[editing ? 'Actualizar paciente' : 'Registrar paciente']">
		</form>
	</div>
</template>