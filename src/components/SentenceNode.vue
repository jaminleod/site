<script setup lang="ts">
	import {ref} from 'vue'

	export type SentenceLink = {text: string, link?: string}
	export type SentenceOption = string | SentenceLink | SentenceTree
	export type SentenceTree = {[key: string]: SentenceOption[]}

	const props = defineProps<{node: SentenceTree | (string | SentenceTree)[], depth?: number}>()

	function randomIndex(max: number) {
		return Math.floor(Math.random() * max)
	}

	function randomPickAvoid<T>(arr: T[], avoid?: T): T {
		if (arr.length <= 1) return arr[0]
		if (avoid === undefined) return arr[randomIndex(arr.length)]

		const filtered = arr.filter(x => x !== avoid)
		if (filtered.length === 0) {
			return arr[randomIndex(arr.length)]
		}

		return filtered[randomIndex(filtered.length)]
	}

	const currentDisplay = ref<string>('')
	const currentLink = ref<string | null>(null)
	const currentChild = ref<any>(null)
	const previousDisplay = ref<string | null>(null)
	const childKey = ref(0)

	function regenerate() {
		const n = props.node

		if (Array.isArray(n)) {
			const options = n.map(opt => {
				if(typeof opt === 'string') return opt
				if('text' in opt) return opt.text
				return Object.keys(opt)[0]
			})

			const chosenText = randomPickAvoid(options, previousDisplay.value ?? undefined)

			const chosen = n.find(opt => {
				if (typeof opt === 'string') return opt === chosenText
				if ('text' in opt) return opt.text === chosenText
				return Object.keys(opt)[0] === chosenText
			})

			if (!chosen) return

			if (typeof chosen === 'string') {
				currentDisplay.value = chosen
				currentLink.value = null
				currentChild.value = null
			}
			else if (typeof chosen === 'object' && 'text' in chosen) {
				const optionObj = chosen as SentenceOption
				const linkObj = optionObj as SentenceLink
				currentDisplay.value = linkObj.text
				currentLink.value = linkObj.link ?? null
				currentChild.value = null
			}
			else {
				const key = Object.keys(chosen)[0]
				currentDisplay.value = key
				currentLink.value = null
				currentChild.value = chosen[key]
			}
		}
		else {
			const keys= Object.keys(n)
			const key = randomPickAvoid(keys, previousDisplay.value ?? undefined)
			currentDisplay.value = key
			currentLink.value = null
			currentChild.value = n[key]
		}

		previousDisplay.value = currentDisplay.value
		childKey.value++
	}

	regenerate()
</script>

<script lang="ts">
	export default {
		name: 'SentenceNode'
	}
</script>

<template>
		<a v-if="currentLink" :href="currentLink" @contextmenu.prevent="regenerate">{{currentDisplay}}</a>
		<button v-else aria-label="Change sentence fragment" @click="regenerate" @contextmenu.prevent="regenerate">
			{{currentDisplay}}
		</button>
		<SentenceNode v-if="currentChild" :node="currentChild" :key="childKey" :depth="(depth ?? 0) + 1"/>
</template>

<style scoped>
	button {
		text-decoration-style: dotted;
		margin-right: 0.225rem;
	}

	@media (hover: hover) and (pointer: fine) {
		a:hover, button:hover {
			text-decoration-style: wavy;
		}
	}
</style>

