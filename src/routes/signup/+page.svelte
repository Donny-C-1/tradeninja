<script>
	import '$lib/styles/app.css';
	import sidePic from '$lib/images/pic2.jpg';
	import Loader from '$lib/components/LoaderSpinningCircles.svelte';
	import TextInput from '$lib/components/FormTextInput.svelte';
	import PasswordInput from '$lib/components/FormPasswordInput.svelte';
    import Snackbar from '$lib/components/Snackbar.svelte';
	import { PUBLIC_API_URL } from '$env/static/public';
	import { goto } from '$app/navigation';

	let isLoading = $state(false);
	let email = $state('');
	let password = $state('');
	let apiKey = $state('');
	let secretKey = $state('');
	let rememberMe = $state(false);
    let isSnackbarVisible = $state(false);
    let serverError = $state("");

	async function onsubmit(evt) {
		evt.preventDefault();
        if (isLoading === true) return;
        isLoading = true;

        const formData = { email, password, apiKey, secretKey, rememberMe };

        try {
            const response = await fetch(`${PUBLIC_API_URL}/api/auth/signup`, {
                method: 'post',
                body: JSON.stringify(formData),
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            if (response.ok) return goto("/dashboard");

            const data = await response.json();
            serverError = data.message;
            console.log('DATA', data);
        } catch (err) {
            console.error('ERROR', err);
        } finally {
            isLoading = false;
            isSnackbarVisible = true;
            setTimeout(() => isSnackbarVisible = false, 3500);
        }
	}
</script>

<svelte:head>
	<title>Sign Up</title>
	<meta
		name="description"
		content="Create an account to access exclusive features and stay updated with the latest news and offers from TradeNinja. Sign up now to start your journey with us."
	/>
</svelte:head>

<div class="body">
	<div class="left">
		<div class="card">
			<h1>Trade Ninja</h1>
			<p class="subtext">Make Money. Save Time</p>
			<div class="link_group">
				<a href="javscript:" class="signup active" tabindex="-1">Sign Up</a>
				<a href="/login" class="signup">Log In</a>
			</div>
			<form action="" method="post" {onsubmit}>
				<h2>Join Us Today!</h2>
				<p>Sign up to start trading</p>
				<br />
				<TextInput
					type="email"
					name="email"
					id="email"
					autocomplete="email"
					required="true"
					placeholder="Email"
					bind:value={email}
				/>
				<br />
				<PasswordInput
					type="password"
					name="password"
					id="password"
					required="true"
					placeholder="Password"
					autocomplete="current-password"
					pattern={'(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#$%^&*_+])[a-z0-9A-Z!@#$%^&*_ +]{8,64}'}
					minlength="8"
					maxlength="64"
					title="At least 1 uppercase, 1 lowercase, 1 number, 1 symbol"
					bind:value={password}
				/>
				<br />
				<PasswordInput
					type="password"
					name="apiKey"
					id="apiKey"
					required="true"
					placeholder="Api Key"
					minlength="20"
					maxlength="20"
					bind:value={apiKey}
				/>
				<br />
				<PasswordInput
					type="password"
					name="secretKey"
					id="secretKey"
					required="true"
					placeholder="Secret Key"
					bind:value={secretKey}
				/>
				<div class="form_footer">
					<label for="rememberMe">
						<input type="checkbox" name="remember_me" id="remember_me" bind:checked={rememberMe} /> Remember
						Me
					</label>
					<a href="javscript:">Forgot Password?</a>
				</div>
				<input type="submit" value="Sign Up" disabled={isLoading} />
			</form>
		</div>
		{#if isLoading}
			<div class="backdrop">
				<Loader size="50" />
			</div>
		{/if}
	</div>
	<div class="right" style="background-image: url({sidePic})"></div>
</div>

<Snackbar visible={isSnackbarVisible}>{serverError}</Snackbar>

<style>
	.body {
		display: grid;
		grid-template-columns: 1fr;
		min-height: 100vh;

		& .left {
			display: grid;
			place-items: center;
			position: relative;
			padding: 2rem 3rem;

			& .backdrop {
				position: absolute;
				background-color: rgba(0, 0, 0, 0.5);
				inset: 0;
				display: grid;
				place-items: center;
                z-index: 3;
			}
		}

		& .right {
			display: none;
			background-color: cover;
			background-position: center center;
			background-repeat: no-repeat;
		}

		@media screen and (min-width: 38.75rem) {
			grid-template-columns: 1fr 1fr;

			& .left,
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
