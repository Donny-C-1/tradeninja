<script>
	const stats = $state([
		{ label: 'Total Balance', value: '', icon: 'ri-wallet-fill' },
		{ label: 'Daily Profit/Loss', value: '+$1,250.30', icon: 'ri-money-dollar-circle-fill' },
		{ label: 'Open Positions', value: '12', icon: 'ri-folder-open-line' },
		{ label: 'Total Trades', value: '156', icon: 'ri-stack-line' }
	]);

	const assets = [
		{ symbol: 'AAPL', price: '57.499', profit: '+0.24%', positive: true },
		{ symbol: 'GOOGL', price: '2750.8', profit: '-0.15%', positive: false },
		{ symbol: 'MSFT', price: '305.5', profit: '+1.10%', positive: true },
		{ symbol: 'AMZN', price: '3380.2', profit: '+0.75%', positive: true }
	];
</script>

<svelte:head>
	<title>Trade Ninja | Dashboard</title>
	<meta name="description" content="See an overview of all your activities" />
</svelte:head>

<div class="dashboard">
	<header>
		<h1>Dashboard</h1>
	</header>

	<div class="body">
		<section class="stats_grid">
			{#snippet statItem({ label, value, icon })}
				<div class="stats_card">
					<i class={icon}></i>
					<p class="number value">{value}</p>
					<p class="label">{label}</p>
				</div>
			{/snippet}

			{#each stats as stat}
				{@render statItem(stat)}
			{/each}
		</section>

		<section class="widgets_grid">
			<div class="widget">
				<h3>Markets Overview</h3>
				<div class="toggle_switch">
					<input type="radio" name="markets_overview" id="top" class="visually_hidden" checked />
					<label for="top">Popular</label>
					<input type="radio" name="markets_overview" id="new" class="visually_hidden" />
					<label for="new">New Listings</label>
					<div class="inner-bg"></div>
				</div>

				<div class="assets">
					{#snippet asset({ symbol, price, profit, positive })}
						<div class="asset">
							<p class="symbol">{symbol}</p>
							<p class="price number">${price}</p>
							<p class="profit number {positive ? 'positive' : 'negative'}">{profit}</p>
						</div>
					{/snippet}
					{#each assets as assetItem}
						{@render asset(assetItem)}
					{/each}
				</div>
			</div>

			<div class="widget">
				<h3>Transactions</h3>
			</div>

			<div class="widget">
				<h3>Strategies</h3>
			</div>
		</section>
	</div>
</div>

<style>
	.body {
		max-width: 1200px;
		margin: 0 auto;
		padding: 1rem;
	}

	.stats_grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
		gap: 1rem;
		margin-bottom: 1rem;

		& .stats_card {
			display: grid;
			gap: 0.5rem;
			/* background-color: var(--color-surface); */
			padding: 1rem;
			border: 1px solid var(--color-border);
			border-radius: 0.5rem;
			box-shadow: 0 0 1px 1px rgba(0, 0, 0, 0.1);
			color: var(--color-text-secondary);

			& i {
				font-size: 1.5rem;
			}

			& .value {
				font-size: 1.25rem;
				margin: 0;
				color: var(--color-text);
			}
		}
	}

	.widgets_grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
		min-height: 10rem;
		gap: 1rem;

		& .widget {
			padding: 1rem;
			background-color: var(--color-primary);
			border-radius: 0.5rem;
			border: 1px solid var(--color-secondary);

			h3 {
				font-size: 1.25rem;
				font-weight: 500;
			}
		}

		& .toggle_switch {
			--pad-gap: 2px;
			--left: 0%;
			display: flex;
			position: relative;
			align-items: center;
			border-radius: 1.5rem;
			overflow: hidden;
			background-color: var(--color-dark);
			margin-block: 1rem;

			&:has(#new:checked) {
				--left: 50%;
			}

			& label {
				flex: 1;
				text-align: center;
				padding: 1rem 0.75rem;
				cursor: pointer;
				color: var(--color-tertiary);
				background-color: transparent;
				z-index: 1;
				font-size: 0.875rem;
				transition: 0.3s ease;
			}

			& input:checked + label {
				color: var(--color-light);
			}

			&::after {
				content: '';
				position: absolute;
				top: var(--pad-gap);
				left: calc(var(--left) + var(--pad-gap));
				width: calc(50% - var(--pad-gap) - var(--pad-gap));
				height: calc(100% - var(--pad-gap) - var(--pad-gap));
				border-radius: 1.5rem;
				background-color: var(--color-primary);
				transition: 0.3s cubic-bezier(0.25, 1.5, 0.5, 1);
			}

			& .inner-bg {
				display: block;
				content: '';
				position: absolute;
				width: 4rem;
				height: 90%;
				border-radius: 1.5rem;
				left: var(--left);
				background-color: var(--color-primary);
				transition: 0.3s ease;
				z-index: -1;
			}
		}

		& .assets {
			display: grid;
			gap: 0.5rem;

			& .asset {
				display: flex;
				justify-content: space-between;
				padding: 0.75rem;
				border: 1px solid var(--color-border);
				border-radius: 25rem;
				box-shadow: 0 0 4px rgba(10, 10, 10, 1);

				.symbol,
				.profit {
					color: var(--color-tertiary);
					font-size: 0.875rem;
				}

				.positive {
					color: var(--color-success);
				}

				.negative {
					color: var(--color-error);
				}

				.price {
					margin-inline: auto 2rem;
				}
			}
		}
	}
</style>
