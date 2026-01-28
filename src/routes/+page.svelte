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

    feedback = 'Kiitos kiinnostuksestasi! Saat sähköpostiisi ilmoituksen, kun Beta-versio on testattavissa ✅';
    success = true;
  };
</script>

<main class="mx-auto w-full max-w-5xl space-y-10 px-5 pb-20 pt-12 font-['Manrope'] sm:pt-16 sm:space-y-12">
      <section class="space-y-6">
        <div class="space-y-4">
          <h1 class="max-w-xl font-['Fraunces'] text-4xl tracking-tight sm:text-5xl">
            Lopeta mailan arpominen.
          </h1>
          <p class="max-w-2xl text-base leading-relaxed text-zinc-600 sm:text-lg">
            PocketCaddie antaa jokaiselle lyönnille nopean mailasuosituksen ja auttaa tunnistamaan sun yleisimmät missit.
          </p>
        </div>

        <div class="rounded-[36px] bg-white p-6 shadow-[0_24px_60px_-38px_rgba(24,24,27,0.22)] sm:p-7">
          <form class="flex flex-col gap-4" on:submit|preventDefault={handleSubmit}>
            <label class="sr-only" for="email">Sähköposti</label>
            <input
              id="email"
              type="email"
              placeholder="Sähköposti"
              class="w-full rounded-2xl border border-zinc-200 bg-white px-5 py-4 text-base text-zinc-900 shadow-sm outline-none transition placeholder:text-zinc-400 focus:border-lime-300 focus:ring-2 focus:ring-lime-200"
              bind:value={email}
            />
            <button
              type="submit"
              class="rounded-2xl bg-lime-300 px-5 py-4 text-base font-semibold text-zinc-900 shadow-lg shadow-lime-500/30 transition hover:-translate-y-0.5 hover:bg-lime-200 hover:shadow-lime-400/40 active:translate-y-0 active:scale-[0.99] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-lime-300"
            >
              Liity jonoon
            </button>
          </form>
          <div class="space-y-4 pt-3 text-xs text-zinc-500">
            <p>Lähetän vain julkaisu- ja beta-kutsut. Ei spämmiä. </p>
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
            
          </div>
        </div>
      </section>

      <section>
        <div class="mx-auto w-full max-w-md sm:max-w-lg md:max-w-xl lg:max-w-2xl">
          <div class="grid grid-cols-2 gap-4 sm:gap-6">
            <img
              src="/app-shot-1.png"
              alt="MyCaddie sovelluksen näkymä"
              class="h-auto w-full rounded-[28px] shadow-[0_22px_55px_-35px_rgba(24,24,27,0.25)]"
              loading="lazy"
            />
            <img
              src="/app-shot-2.png"
              alt="MyCaddie analytiikkanäkymä"
              class="h-auto w-full rounded-[28px] shadow-[0_22px_55px_-35px_rgba(24,24,27,0.25)]"
              loading="lazy"
            />
          </div>
          <p class="text-xs text-zinc-400">*Suomenkieliset demokuvat tulossa pian!</p>
        </div>
      </section>

      <section>
        <ul class="grid gap-6 md:grid-cols-1 lg:grid-cols-3">
          <li class="rounded-4xl bg-lime-50/70 p-6 shadow-[0_22px_55px_-35px_rgba(24,24,27,0.22)]">
            <div class="flex h-12 w-12 items-center justify-center rounded-2xl bg-lime-200 text-zinc-900">
              <img src="/1386676.png" alt="Golfbägi" class="h-6 w-6" loading="lazy" />
            </div>
            <h3 class="mt-5 text-xl font-semibold text-zinc-900">Oma bägi</h3>
            <p class="mt-3 text-sm leading-relaxed text-zinc-600">
              Syötä lyöntimittasi vain kerran. PocketCaddie oppii bägisi ja muistaa sen puolestasi.
            </p>
          </li>
          <li class="rounded-4xl bg-lime-50/70 p-6 shadow-[0_22px_55px_-35px_rgba(24,24,27,0.22)]">
            <div class="flex h-12 w-12 items-center justify-center rounded-2xl bg-lime-200 text-zinc-900">
              <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M13 2 4 14h6l-1 8 9-12h-6l1-8Z" />
              </svg>
            </div>
            <h3 class="mt-5 text-xl font-semibold text-zinc-900">Mailasuositus</h3>
            <p class="mt-3 text-sm leading-relaxed text-zinc-600">
              Saat tarkan mailasuosituksen sekunneissa – kuin oma caddie taskussasi.
            </p>
          </li>
          <li class="rounded-4xl bg-lime-50/70 p-6 shadow-[0_22px_55px_-35px_rgba(24,24,27,0.22)]">
            <div class="flex h-12 w-12 items-center justify-center rounded-2xl bg-lime-200 text-zinc-900">
              <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M4 18V9" />
                <path d="M10 18V5" />
                <path d="M16 18v-7" />
                <path d="M22 18v-4" />
              </svg>
            </div>
            <h3 class="mt-5 text-xl font-semibold text-zinc-900">Analytiikka</h3>
            <p class="mt-3 text-sm leading-relaxed text-zinc-600">
              Näet mihin yleisimmin missaat ja kehityt kierros kierrokselta.
            </p>
          </li>
        </ul>
      </section>
</main>
