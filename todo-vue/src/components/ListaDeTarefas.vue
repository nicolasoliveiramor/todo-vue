<script setup>
	const props = defineProps(['tarefas'])
</script>

<template>
	<!-- Lista de tarefas filtradas -->
	<span v-if="props.tarefas.length === 0" class="col-md-12 d-flex justify-content-center p-5">
		Nenhuma tarefa disponivel
	</span>
    <ul v-else class="list-group mt-4">
        <!-- Itera sobre as tarefas filtradas -->
        <li v-for="tarefa in props.tarefas" class="list-group-item">
			<!-- Checkbox para marcar/desmarcar como finalizada -->
			<input @change="evento => tarefa.finalizada = evento.target.checked" v-model="tarefa.finalizada" type="checkbox" id="tarefa.titulo" :key="tarefa.finalizada" >

        	<!-- Label com estilo baseado no estado 'finalizada' -->
			<label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo" >
				{{ tarefa.titulo }}
			</label>
    	</li>
    </ul>
</template>	

<style scoped>
	.done {
    	text-decoration: line-through;
	}
</style>