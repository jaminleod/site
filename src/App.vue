<script setup type="ts">
	import {ThemeButton} from '@jaminleod/theme'
	import Header from './components/Header.vue'
	import Describer from './components/Describer.vue'
	import BioTree from './data/bio.ts'
</script>

<template>
	<div class="site">
		<Header/>
		<p>
		<Describer :tree="BioTree"/>
		</p>

		<hr>

		<nav>
			<a href="https://youtube.com/@jaminleod">Youtube</a>
			<a href="https://github.com/jaminleod">Github</a>
			<a href="https://x.com/jaminleod">Twitter</a>
			<a href="https://roblox.com/users/136167232/profile">Roblox</a>
			<a href="https://devforum.roblox.com/u/bblqdes/summary">Devforum</a>
			<hr>
			<ThemeButton/>
		</nav>
	</div>
</template>

<style scoped>
	.pronounce {
		opacity: 0.5;
	}

	.site {
		display: flex;
		justify-content: center;
		flex-direction: column;
		gap: 1.2rem;
		padding: 2rem 3rem;
		margin: 0 auto;
		max-width: var(--jml-max-width);
		min-height: 100vh;
	}

	.site a {
		width: fit-content;
	}

	.theme-button {
		width: fit-content;
	}

	hr {
		border: none;
		border-top: 1px solid var(--jml-color-border);
		margin: 0;
		width: 50%;
		opacity: 0.1;
	}

	p {
		margin: 0;
	}

	nav {
		display: flex;
		gap: 1.2rem;
		justify-content: center;
		flex-direction: column;
	}

	@media (max-width: var(--jml-max-width)) {
		.site {
			padding: 2rem 2rem;
		}
	}

</style>
