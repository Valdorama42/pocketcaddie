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
  import TallySignupEmbed from '$lib/components/landingpage/TallySignupEmbed.svelte';
  import EmailSignupForm from '$lib/components/landingpage/EmailSignupForm.svelte';

  const SIGNUP_VARIANT = 'tally'; // 'tally' or 'local'
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

        {#if SIGNUP_VARIANT === 'tally'}
          <TallySignupEmbed />
        {:else}
          <EmailSignupForm />
        {/if}
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
