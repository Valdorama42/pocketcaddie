<svelte:head>
  <title>PocketCaddie — Tulossa pian</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;700&family=Manrope:wght@400;500;600;700&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<script>
  let email = '';
  let feedback = '';
  let success = false;

  const cardClass =
    'rounded-2xl border border-zinc-200/80 bg-white/90 shadow-[0_25px_60px_-40px_rgba(24,24,27,0.45)] backdrop-blur';
  const formCardClass = `${cardClass} border-lime-200/70 ring-1 ring-lime-100/70`;
  const featureCardClass =
    'relative overflow-hidden rounded-2xl border border-zinc-200/80 bg-zinc-50/90 text-zinc-900 shadow-[0_25px_60px_-45px_rgba(24,24,27,0.35)]';

  const handleSubmit = () => {
    const trimmed = email.trim();

    if (!trimmed || !trimmed.includes('@')) {
      feedback = 'Syötä toimiva sähköposti.';
      success = false;
      return;
    }

    feedback = 'Jes! Olet jonossa — nähdään betassa ✅';
    success = true;
  };
</script>

<div
  class="min-h-screen bg-white text-zinc-900 [background:radial-gradient(1200px_500px_at_20%_-10%,rgba(190,242,100,0.22),transparent),radial-gradient(900px_500px_at_100%_0%,rgba(24,24,27,0.06),transparent)]"
>
  <div class="mx-auto w-full max-w-5xl px-5 pb-20 pt-10 font-['Manrope'] sm:pt-16">
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

    <main class="mt-12 space-y-10 sm:mt-16 sm:space-y-12">
      <section class="space-y-6">
        <div class="space-y-4">
          <h1 class="max-w-xl font-['Fraunces'] text-4xl tracking-tight sm:text-5xl">
            Lopeta mailan arpominen.
          </h1>
          <p class="max-w-2xl text-base leading-relaxed text-zinc-600 sm:text-lg">
            Syötä mailojen kantamat kerran. Saat jokaiselle lyönnille nopean
            mailasuosituksen ja näet sun yleisimmät missit.
          </p>
        </div>

        <div class={`${formCardClass} space-y-4 p-4 sm:p-5`}>
          <form class="flex flex-col gap-3 sm:flex-row" on:submit|preventDefault={handleSubmit}>
            <label class="sr-only" for="email">Sähköposti</label>
            <input
              id="email"
              type="email"
              placeholder="Sähköposti"
              class="w-full flex-1 rounded-2xl border border-zinc-900/15 bg-white px-4 py-3 text-sm font-['Manrope'] text-zinc-900 shadow-sm outline-none transition placeholder:text-zinc-500 focus:border-lime-500 focus:ring-2 focus:ring-lime-200"
              bind:value={email}
            />
            <button
              type="submit"
              class="group relative overflow-hidden rounded-2xl bg-lime-300 px-5 py-3 text-sm font-['Manrope'] font-medium text-zinc-900 shadow-lg shadow-lime-500/35 transition hover:-translate-y-0.5 hover:bg-lime-200 hover:shadow-lime-400/50 active:translate-y-0 active:scale-[0.99] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-lime-300"
            >
              <span class="relative z-10">Liity jonoon</span>
              <span
                class="absolute inset-0 -translate-x-full bg-[linear-gradient(110deg,transparent,rgba(255,255,255,0.65),transparent)] transition-transform duration-700 group-hover:translate-x-full"
              ></span>
            </button>
          </form>
          <div class="space-y-3 text-xs">
            <p class="text-zinc-500">Ei spämmiä. Lähetän vain julkaisu- ja beta-kutsut.</p>
            {#if feedback}
              <div
                class={`inline-flex items-center gap-2 rounded-full border px-3 py-1 text-xs font-medium ${
                  success
                    ? 'border-lime-200 bg-lime-100 text-lime-900'
                    : 'border-rose-200 bg-rose-100 text-rose-700'
                }`}
                aria-live="polite"
              >
                {feedback}
              </div>
            {/if}
            <div class="flex items-center gap-3 text-zinc-600">
              <div class="flex -space-x-2">
                <span class="inline-flex h-6 w-6 rounded-full border border-white bg-zinc-200"></span>
                <span class="inline-flex h-6 w-6 rounded-full border border-white bg-lime-200"></span>
                <span class="inline-flex h-6 w-6 rounded-full border border-white bg-zinc-300"></span>
              </div>
              <span>25+ golfaria jonossa</span>
            </div>
            <div class="space-y-2">
              <p class="text-[10px] font-medium uppercase tracking-[0.2em] text-zinc-500">
                Julkaisuun
              </p>
              <div class="grid grid-cols-4 gap-3 rounded-2xl border border-zinc-200/70 bg-white/80 p-3 text-center">
                <div>
                  <p class="text-lg font-medium text-zinc-900">24</p>
                  <p class="text-[10px] uppercase tracking-[0.2em] text-zinc-500">päivää</p>
                </div>
                <div>
                  <p class="text-lg font-medium text-zinc-900">08</p>
                  <p class="text-[10px] uppercase tracking-[0.2em] text-zinc-500">tuntia</p>
                </div>
                <div>
                  <p class="text-lg font-medium text-zinc-900">12</p>
                  <p class="text-[10px] uppercase tracking-[0.2em] text-zinc-500">min</p>
                </div>
                <div>
                  <p class="text-lg font-medium text-zinc-900">04</p>
                  <p class="text-[10px] uppercase tracking-[0.2em] text-zinc-500">sek</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section>
        <ul class="grid gap-4 sm:grid-cols-3">
          <li class={`${featureCardClass}`}>
            <div class="absolute right-0 top-0 h-24 w-24 translate-x-8 -translate-y-8 rounded-full bg-lime-200/60 blur-2xl"></div>
            <div class="space-y-4 p-5">
              <div class="flex items-center gap-3">
                <span
                  class="flex h-10 w-10 items-center justify-center rounded-xl bg-lime-200 text-lg text-zinc-900"
                >
                  📏
                </span>
                <span class="text-xs font-medium uppercase tracking-[0.2em] text-zinc-500">
                  Kantamat
                </span>
              </div>
              <p class="text-lg font-medium leading-tight">
                Metrit sisään, maila ulos
              </p>
              <p class="text-sm text-zinc-600">
                Toimii metreillä ja antaa suosituksen sekunneissa.
              </p>
            </div>
            <div class="h-2 w-full bg-lime-300"></div>
          </li>
          <li class={`${featureCardClass}`}>
            <div class="absolute right-0 top-0 h-24 w-24 translate-x-8 -translate-y-8 rounded-full bg-lime-200/60 blur-2xl"></div>
            <div class="space-y-4 p-5">
              <div class="flex items-center gap-3">
                <span
                  class="flex h-10 w-10 items-center justify-center rounded-xl bg-lime-200 text-lg text-zinc-900"
                >
                  ⏱️
                </span>
                <span class="text-xs font-medium uppercase tracking-[0.2em] text-zinc-500">
                  Rutiini
                </span>
              </div>
              <p class="text-lg font-medium leading-tight">
                10 sekunnin rutiini ennen lyöntiä
              </p>
              <p class="text-sm text-zinc-600">
                Nopea muistilista: olosuhteet, makuu, ja oma lyöntikunto yhden napin painalluksella.
              </p>
            </div>
            <div class="h-2 w-full bg-lime-300"></div>
          </li>
          <li class={`${featureCardClass}`}>
            <div class="absolute right-0 top-0 h-24 w-24 translate-x-8 -translate-y-8 rounded-full bg-lime-200/60 blur-2xl"></div>
            <div class="space-y-4 p-5">
              <div class="flex items-center gap-3">
                <span
                  class="flex h-10 w-10 items-center justify-center rounded-xl bg-lime-200 text-lg text-zinc-900"
                >
                  🎯
                </span>
                <span class="text-xs font-medium uppercase tracking-[0.2em] text-zinc-500">
                  Missit
                </span>
              </div>
              <p class="text-lg font-medium leading-tight">
                Missit talteen, kehitys näkyviin
              </p>
              <p class="text-sm text-zinc-600">
                Lyhyt / pitkä / vasen / oikea / griini — kaikki muistiin.
              </p>
            </div>
            <div class="h-2 w-full bg-lime-300"></div>
          </li>
        </ul>
      </section>

      <section
        class={`${cardClass} p-4 sm:p-6`}
      >
        <div
          class="flex aspect-video w-full flex-col items-center justify-center gap-3 rounded-xl border border-dashed border-zinc-300/90 bg-[linear-gradient(135deg,rgba(244,244,245,0.7),rgba(255,255,255,0.2))] px-4 text-center"
        >
          <div class="flex h-12 w-12 items-center justify-center rounded-full bg-lime-100 text-lime-800">
            <span class="text-lg font-semibold">▶</span>
          </div>
          <div class="space-y-1 text-sm text-zinc-600">
            <p class="font-medium text-zinc-700">Tuleva näkymä</p>
            <p>Tähän tulee sovelluskuvia tai lyhyt demo ennen julkaisua.</p>
          </div>
        </div>
      </section>
    </main>
  </div>
</div>
