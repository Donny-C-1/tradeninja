<script>
	import '$lib/styles/app.css';
	import TextInput from '$lib/components/FormTextInput.svelte';
	import PasswordInput from '$lib/components/FormPasswordInput.svelte';
	import pic1 from '$lib/images/pic1.jpg';
	import Loader from '$lib/components/LoaderSpinningCircles.svelte';
	import { PUBLIC_API_URL } from '$env/static/public';
	import { goto } from '$app/navigation';

	let email = $state('');
	let password = $state('');
	let rememberMe = $state(false);
	let isLoading = $state(false);

	async function onsubmit(evt) {
		evt.preventDefault();
		if (isLoading === true) return;

		isLoading = true;

		const formData = { email, password, rememberMe };
		console.log(formData);

		try {
			const response = await fetch(`${PUBLIC_API_URL}/api/auth/login`, {
				method: 'post',
				body: JSON.stringify(formData),
				headers: {
					'Content-Type': 'application/json'
				}
			});
			if (response.ok) return goto("/dashboard");

			const data = await response.json();
			console.log('DATA', data);
			goto("/dashboard");
		} catch (err) {
			console.error('ERROR', err);
		} finally {
			isLoading = false;
		}
	}
</script>

<div class="body">
	<div class="left">
		<div class="card">
			<h1>Trade Ninja</h1>
			<p class="subtext">Make Money. Save Time</p>
			<div class="link_group">
				<a href="/signup" class="signup">Sign Up</a>
				<a href="javscript:" class="login active" tabindex="-1">Log In</a>
			</div>
			<form action="" {onsubmit}>
				<h2>Welcome Back</h2>
				<p>Log in to start trading</p>
				<br />
				<TextInput
					type="email"
					name="email"
					id="email"
					autocomplete="email"
					required
					placeholder="Email"
					autofocus="true"
					bind:value={email}
				/>
				<br />
				<PasswordInput
					type="password"
					name="password"
					id="password"
					required
					placeholder="Password"
					autocomplete="current-password"
					pattern={'(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#$%^&*_+])[a-z0-9A-Z!@#$%^&*_ +]{8,64}'}
					minlength="8"
					maxlength="64"
					title="Password must be 8-20 characters long and include at least one uppercase letter, one lowercase letter, one number, and one special character."
					bind:value={password}
				/>
				<div class="form_footer">
					<label>
						<input type="checkbox" name="remember_me" id="remember_me" bind:checked={rememberMe} />
						Remember me
					</label>
					<a href="javscript:">Forgot Password?</a>
				</div>
				<input type="submit" value="Log In" disabled={isLoading} />
			</form>
		</div>
		{#if isLoading}
			<div class="backdrop">
				<Loader size="50" />
			</div>
		{/if}
	</div>
	<div class="right" style="background-image: url({pic1})"></div>
</div>

<style>
	.body {
		display: grid;
		grid-template-columns: 1fr;
		min-height: 100vh;

		& .left {
			display: grid;
			position: relative;
			place-items: center;
			padding: 2rem 3rem 2rem;

			& .backdrop {
				position: absolute;
				background-color: rgba(0, 0, 0, .5);
				inset: 0;
				display: grid;
				place-items: center;
			}
		}

		& .right {
			display: none;
			background-size: cover;
			background-position: center;
			background-repeat: no-repeat;
		}

		@media screen and (min-width: 38.75rem) {
			grid-template-columns: 1fr 1fr;

			& .left {
				display: block;
			}

			& .right {
				display: block;
			}
		}
	}

	.card {
		padding: 1rem;
		max-width: 25rem;
		margin-inline: auto;

		& h1 {
			text-align: center;
			font-size: 3rem;
			font-weight: lighter;
		}
		& .subtext {
			margin-bottom: 1rem;
			text-align: center;
			font-size: large;
			font-family: var(--color-text-secondary);
		}

		& form {
			margin-top: 3rem;
			& h2 {
				font-size: 2rem;
				font-weight: normal;
			}
		}

		& .link_group {
			display: flex;
			gap: 1rem;

			& a:is(:link, :visited) {
				flex: 1;
				display: block;
				padding: 0.75rem;
				text-align: center;
				color: var(--color-text);
				background-color: transparent;
				border: 2px solid currentColor;
				border-radius: 0.75em;
				text-decoration-line: none;
				transition: 0.3s ease;

				&:is(:active, :hover, :focus-visible) {
					color: var(--color-primary);
				}

				&.active {
					background-color: var(--color-primary);
					color: var(--color-background);
				}
			}
		}

		& .form_footer {
			display: flex;
			justify-content: space-between;
			margin-block: 0.5rem 1.5rem;

			& a {
				color: var(--color-text);
				text-decoration-color: transparent;
				transition: 0.3s ease;

				&:is(:hover, :focus-visible) {
					color: var(--color-primary);
					text-decoration-color: currentColor;
				}
			}
			& label {
				cursor: pointer;
				transition: 0.3s ease;

				& input {
					accent-color: var(--color-primary);
				}

				&:has(input:is(:hover, :focus-visible)) {
					color: var(--color-primary);
				}
			}
		}

		& input[type='submit'] {
			width: 100%;
			background-color: var(--color-primary);
			padding: 1rem;
			border: 0;
			border-radius: 0.5rem;
			color: var(--color-light);
			transition: 0.3s ease;
			cursor: pointer;

			&:is(:hover, :focus-visible) {
				background-image: linear-gradient(
					to top,
					color-mix(in srgb, var(--color-primary) 80%, black 20%),
					var(--color-primary, red) 70%
				);
			}
		}
	}
</style>
