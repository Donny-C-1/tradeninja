<script>
	import "$lib/styles/app.css";
	import "remixicon/fonts/remixicon.css";
	import { onMount } from "svelte";
    import heroImg from "$lib/images/hero_image.png";

	let isMenuVisible = $state(false);
	function toggleMenu() {
		isMenuVisible = !isMenuVisible;
	}
</script>

<svelte:head>
	<title>Trade Ninja</title>
	<meta name="description" content="Trade Ninja - Automate your trading strategies with precision and efficiency. Empowering you to make smarter decisions and maximize your profits effortlessly. Start trading with 24/7 market analysis, 100+ technical indicators, and 200+ trading pairs."/>
</svelte:head>

<header>
	<div class="navbar">
		<div class="logo">TradeNinja</div>
	</div>
	<nav>
		<ul class:visible={isMenuVisible}>
			<li><a href="#hero">Home</a></li>
			<li><a href="#about">About</a></li>
			<li><a href="#features">Features</a></li>
			<li><a href="#contact">Contact</a></li>
		</ul>
		<a href="/signup">Get Started</a>
		<button
			aria-label="toggle nav"
			class="menu_button"
			class:active={isMenuVisible}
			onclick={toggleMenu}
		>
			<div></div>
			<div></div>
			<div></div>
		</button>
	</nav>
</header>

<div class="hero_section" style:--bgImg="url('{heroImg}')">
	<div class="text_box">
		<h1>Trade <svg width="40" height="40"><use href="#arrow"/></svg> <br/><span class="accent">Smarter</span>, Not Harder</h1>
		<p class="subtext">Automate your trading strategies with precision and efficiency. Trade Ninja empowers you to make smarter decisions and maximize your profits effortlessly.</p>
		<a href="/login" class="call_to_action">Start Trading</a>
		<div class="info">
			<div>
				<p>24/7</p>
				<p>Market Analysis and Trade Execution</p>
			</div>
			<svg width="3" height="25"><use href="#vertical-line"/></svg>
			<div>
				<p>100+</p>
				<p>Technical Indicators</p>
			</div>
			<svg width="3" height="25"><use href="#vertical-line"/></svg>
			<div>
				<p>200+</p>
				<p>Trading Pairs</p>
			</div>
		</div>
	</div>
</div>

<svg class="symbols">
	<symbol viewBox="0 0 512 512" stroke="currentColor" id="arrow">
		<polyline points="112 244 256 100 400 244" style="fill:none;stroke-linecap:round;stroke-linejoin:round;stroke-width:48px"/>
		<line x1="256" y1="120" x2="256" y2="412" style="fill:none;stroke-linecap:round;stroke-linejoin:round;stroke-width:48px"/>
	</symbol>
	<symbol viewBox="0 0 3 25" stroke="currentColor" stroke-linecap="round" id="vertical-line">
		<line x1="0" y1="0" x2="0" y2="25" stroke-width="3"/>
	</symbol>
</svg>

<style>
	header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		padding: 1rem;
		z-index: 5;

		& .logo {
			font-size: large;
			font-weight: bold;
		}

		& nav {
			display: flex;
			gap: 1em;

			& ul {
				display: none;
				flex-direction: column;
				align-items: center;
				gap: 2em;
				padding-block: 1rem;
				border-radius: 1rem;
				position: absolute;
				right: 2rem;
				left: 2rem;
				top: 100%;
				list-style-type: none;
				background-color: rgba(255, 255, 255, .1);
				backdrop-filter: blur(15px);
				box-shadow: 0 0 .5rem .1rem rgba(0, 0, 0, 0.25);
				z-index: 5;

				&.visible {
					display: flex;
				}

				& a {
					color: currentColor;
					transition: .3s ease;
					text-decoration-line: underline;
					text-decoration-color: transparent;

					&:hover {
						color: var(--color-primary);
						text-decoration-color: currentColor;
					}
				}

				@media screen and (min-width: 35rem) {
					width: 18.75rem;
					left: unset;
				}

				@media screen and (min-width: 650px) {
					display: flex;
					flex-direction: row;
					left: 50%;
					top: 50%;
					translate: -50% -50%;
					background-color: transparent;
					backdrop-filter: unset;
					box-shadow: unset;
				}
			}

			& button {
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				border: 0;
				background-color: transparent;
				font-size: 1.5rem;
				cursor: pointer;

				&.active {
					color: var(--color-danger);

					& div:first-child {
						rotate: 45deg;
						translate: 0 0.5rem;
					}

					& div:nth-child(2) {
						display: none;
					}

					& div:last-child {
						rotate: -45deg;
						translate: 0 -0.5rem;
					}
				}

				& div {
					width: 1.5rem;
					height: 0.2rem;
					background-color: currentColor;
					border-radius: 0.5rem;
					transition: 0.3s ease;
				}

				@media screen and (min-width: 40.625rem) {
					display: none;
				}
			}
		}
	}

	.hero_section {
		min-height: 100vh;
		background-color: var(--color-tertiary);
		background-image: var(--bgImg), radial-gradient(30rem at 80% -5%, var(--color-secondaryf, #d7dffd) 60%, transparent), radial-gradient(20rem at 100% 100%, var(--color-secondaryf, #d7dffd) 50%, transparent), radial-gradient(ellipse 40rem 20rem at 10% 100%, var(--color-accent) 50%, transparent);
		background-size: contain;
		background-position: center right;
		background-repeat: no-repeat;

		& .text_box {
			height: 100%;
			max-width: 40rem;
			padding: 6rem 3rem 1rem;
			/* background-color: red; */

			& h1 {
				font-size: 4rem;

				& .accent {
					color: var(--color-primary);
				}

				& svg {
					color: var(--color-light);
					background-color: var(--color-primary);
					border-radius: 50%;
					rotate: 45deg;
					padding: .5rem;
					translate: 0 -.25rem;
					box-shadow: .1rem 0rem .1rem .1rem rgba(224, 134, 232, 0.25);
				}
			}

			& .subtext {
				line-height: 1.5;
				color: var(--color-text-secondary);
			}

			& .call_to_action {
				display: inline-block;
				padding: 1em 1.4em;
				font-size: .875rem;
				border-radius: 1.7rem;
				background-color: var(--color-primary);
				color: var(--color-light);
				border-style: solid;
				border-color: color-mix(in srgb, var(--color-primary) 10%, white 80%);
				box-shadow: 0 0 .3rem .1rem rgba(78, 1, 81, 0.25);
				cursor: pointer;
				margin-top: 1.5rem;
				margin-bottom: 2rem;
				text-decoration-line: none;
			}

			& .info {
				display: flex;
				align-items: center;
				gap: 2rem;

				& svg {
					color: var(--color-text-secondary);
				}

				& p:first-child {
					font-weight: bold;
					font-family: "Noto Sans", Rockwell;
					margin-bottom: .25rem;
				}

				& p:last-child {
					font-size: .875rem;
					color: var(--color-text-secondary);
				}
			}
		}
	}


.symbols {
	display: none;
}
</style>