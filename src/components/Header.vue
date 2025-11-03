<script setup lang="ts">
	import {ref, computed} from 'vue'

	const current = ref(0)
	const titles = ref(["Jaminleod", "Benjamin MacLeod"])
	const pronounciations = ref(["/jah-min-loud/", "/ben-jah-min/ · /mah-cloud/"])
	const title = computed(() =>
		titles.value[current.value]
	)
	const pronounced = computed(() =>
		pronounciations.value[current.value]
	)

	function toggle() {
		current.value = (current.value + 1) % titles.value.length
	}
</script>

<template>
	<div class="header">
		<button @click="toggle">
			<h1>{{title}}</h1>
		</button>
		<p class="pronounce">{{pronounced}}</p>
	</div>
</template>

<style scoped>
	.header {
		user-select: none;
	}

	h1 {
		margin: 0;
	}

	p {
		margin: 0;
		margin-top: 8px;
		opacity: 0.5;
	}
</style>

