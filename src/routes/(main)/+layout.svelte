<script>
	import '$lib/styles/app.css';
	import Sidebar from '$lib/components/Sidebar.svelte';
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { PUBLIC_API_URL } from '$env/static/public';

	const { children } = $props();

	async function checkAuth() {
        return true;
		// const apiUrl = `${PUBLIC_API_URL}/api/auth/check-auth`;
		// const response = await fetch(apiUrl, { credentials: 'include' });

		// if (response.ok) return true;

		// if (response.status === 401) return goto('/login');

		// const data = await response.json();
		// const error = new Error(data.message);
		// error.title = data.title;
		// error.status = response.status;
		// throw error;
	}
</script>

{#await checkAuth()}
	<p>Checking Authentication status</p>
{:then isAuthenticated}
	<div class="app">
		<Sidebar />
		<div class="container">
			{@render children()}
		</div>
	</div>
{:catch err}
	<div>
		<h1>Error {err.status}: {err.title}</h1>
		<p>{err.message}</p>
	</div>
{/await}

<style>
	.app {
		display: flex;
		height: 100vh;
		background-color: var(--color-sidebar-bg);
	}

	.container {
		flex: 1;
		overflow-y: auto;
		max-width: 90rem;
	}
</style>
