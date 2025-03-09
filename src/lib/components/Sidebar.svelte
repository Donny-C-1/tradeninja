<script>
	import 'remixicon/fonts/remixicon.css';
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';

    // * Declare States
    let isOpen = false;

	const menuItems = [
		{ name: 'Dashboard', icon: 'ri-dashboard-fill', path: '/dashboard' },
		{ name: 'Strategies', icon: 'ri-lightbulb-flash-fill', path: '/strategies' },
		{ name: 'Trade Order', icon: 'ri-exchange-fill', path: 'orders' },
		{ name: 'Market Data', icon: 'ri-line-chart-line', path: 'markets' },
		// { name: 'Profile', icon: 'ri-profile-fill', path: '/profile' },
		{ name: 'Logout', icon: 'ri-logout-circle-line', path: '/auth/logout' }
	];

	async function logout(e) {
		e.preventLayout();

		const apiUrl = import.meta.env.VITE_API_URL;

		try {
			await fetch(`${apiUrl}/auth/logout`, { method: 'POST' });
			goto('/auth/login');
		} catch (e) {
			console.log(e);
			alert('Failed to logout');
			// goto("/auth/d")
		}
	}

	onMount(() => {
		const signoutButton = document.querySelector("[href='/auth/logout']");

		signoutButton.addEventListener('click', async (e) => {
			e.preventDefault();

			const apiUrl = import.meta.env.VITE_API_URL;

			try {
				const response = await fetch(`${apiUrl}/auth/logout`, { method: 'POST', credentials: "include" });
				const data = await response.json();
				console.log("RESPONSE: ", response, 'DATA: ', data);
				goto('/auth/login');
			} catch (e) {
				console.log(e);
				alert('Failed to logout');
			}
		});
	});
</script>

<aside class="sidebar" style="--sidebarWidth: {isOpen ? '12rem' : '5.375rem'}">
	<div class="logo">
		<a href="/" aria-label="Logo">Trade Ninja</a>
	</div>
	<nav>
		<ul>
			{#snippet navLink({ name, icon, path })}
				<li>
					<a href={path} class="navlink">
						<i class={icon}></i>
						<span class="nav-text">{name}</span></a
					>
				</li>
			{/snippet}

			{#each menuItems as item}
				{@render navLink(item)}
			{/each}
		</ul>
	</nav>
</aside>

<style>
	.sidebar {
		display: flex;
		flex-direction: column;
		width: var(--sidebar-width);
		transition: width 0.3s ease;
		overflow: hidden;
		border-right: 2px solid var(--color-border);
	}

	.logo {
		padding: 1rem;
		border-bottom: 2px solid var(--color-border);

		& a {
			font-family: var(--font-head);
			font-size: 1.5rem;
			font-weight: 500;
			color: var(--color-text);
			text-decoration: none;
			padding-inline: 1rem;
			color: var(--color-primary);
		}
	}

	nav {
		flex: 1;
	}

	ul {
		display: flex;
		flex-direction: column;
		height: 100%;
		list-style-type: none;
		padding: 1rem;
	}

	li:last-child {
		margin-top: auto;
	}

	.navlink {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		color: var(--color-text);
		padding: 0.75rem;
		margin: 0.25rem;
		border-radius: 0.5rem;
		font-size: 1.25rem;
		text-decoration: none;
		cursor: pointer;
		transition: .3s ease;

		&:hover {
			background-color: var(--color-surface);
			/* color: var(--color-primary); */
			/* box-shadow: 0 0 .1rem .1rem rgba(0, 0, 0, .25); */
		}

		& .nav-text {
			font-size: 1rem;
		}
	}
</style>
