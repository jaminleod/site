<script setup lang="ts">
	import SentenceNode from './SentenceNode.vue'
	type SentenceTree = {[key: string]: (string | SentenceTree)[]}
	defineProps<{tree: SentenceTree}>()
</script>

<template>
	<div class="describer">
		<SentenceNode :node="tree"/>
	</div>
</template>

