<script setup>
// props
const props = defineProps({
	patient: {
		type: Object,
		required: true
	}
})

// emits
defineEmits(['update-patient', 'delete-patient'])
</script>

<template>
	<div class="mx-8 mb-4 bg-white shadow-md px-4 py-8 rounded-xl">
		<p class="font-bold mb-2 text-gray-700 uppercase">ID:
			<span class="font-normal normal-case">
				{{ patient.id }}
			</span>
		</p>
		<p class="font-bold mb-2 text-gray-700 uppercase">Nombre:
			<span class="font-normal normal-case">
				{{ patient.animal }}
			</span>
		</p>
		<p class="font-bold mb-2 text-gray-700 uppercase">Propietario:
			<span class="font-normal normal-case">
				{{ patient.cuidador }}
			</span>
		</p>
		<p class="font-bold mb-2 text-gray-700 uppercase">Email:
			<span class="font-normal normal-case">
				{{ patient.email }}
			</span>
		</p>
		<p class="font-bold mb-2 text-gray-700 uppercase">Fecha Alta:
			<span class="font-normal normal-case">
				{{ patient.alta }}
			</span>
		</p>
		<p class="font-bold mb-2 text-gray-700 uppercase">Síntomas:
			<span class="font-normal normal-case">
				{{ patient.sintomas }}
			</span>
		</p>
		<div class="grid md:grid-cols-2 gap-5 mt-4">
			<button class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg"
				type="button" @click="$emit('delete-patient', patient.id)">Eliminar</button>
			<button
				class="block w-full py-2 px-10 bg-indigo-600 hover:bg-indigo-700 text-white font-bold uppercase rounded-lg"
				type="button" @click="$emit('update-patient', patient.id)">Editar</button>
		</div>
	</div>
</template>