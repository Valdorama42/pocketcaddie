<script>
	import favicon from '$lib/assets/favicon.svg';
	import '../app.css';
	

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<!-- Huom! rgba:n neljäs parametri kuvaa sitä, kuin tumma headerin vihree sävy on! -->
<div
	class="min-h-screen bg-white text-zinc-900 [background:radial-gradient(1200px_500px_at_20%_-10%,rgba(190,242,100,0.35),transparent),radial-gradient(900px_500px_at_100%_0%,rgba(24,24,27,0.06),transparent)]"
>
	<div class="mx-auto w-full max-w-5xl px-5 pt-10 font-['Manrope'] sm:pt-16">
		<nav class="flex flex-wrap items-center justify-between gap-4">
			<div class="flex items-center gap-3">
				<span class="text-lg font-medium tracking-tight">PocketCaddie</span>
				<span
					class="inline-flex items-center gap-2 rounded-full border border-lime-300/70 bg-lime-100/70 px-3 py-1 text-xs font-medium text-lime-900"
				>
					<span class="inline-flex h-2 w-2 rounded-full bg-lime-500"></span>
					Tulossa pian
				</span>
			</div>
		</nav>
	</div>
	{@render children()}
	<footer
		class="w-full border-t border-zinc-200/70 px-5 pb-10 pt-12 text-sm text-zinc-500 [background:linear-gradient(180deg,rgba(255,255,255,0.95)_0%,rgba(255,255,255,0.7)_35%,rgba(190,242,100,0.28)_100%)]"
	>
		<div class="mx-auto w-full max-w-5xl">
			<div class="flex flex-col gap-2 pt-6">
				<p class="font-medium text-zinc-600">PocketCaddie v0.1</p>
				<p>Demo – ominaisuudet ja ulkoasu ovat vielä työn alla.</p>
				<p>© 2026 PocketCaddie. Kaikki oikeudet pidätetään.</p>
			</div>
		</div>
	</footer>
</div>
