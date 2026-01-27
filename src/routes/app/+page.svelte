<svelte:head>
  <title>Oma Caddie — Sovellus</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;700&family=Manrope:wght@400;500;600;700&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<script>
  let distance = '';
  let distanceError = '';
  let activeMenu = null;

  const defaults = {
    temp: '18°C',
    moisture: 'Normaali',
    lie: 'Väylä',
    elevationDirection: 'Tasainen',
    elevationAmount: 'Pieni',
    windDirection: 'Tyyni',
    windStrength: 'Kevyt'
  };

  let selections = { ...defaults };

  const options = {
    temp: ['-5°C', '0°C', '10°C', '18°C', '25°C', '30°C'],
    moisture: ['Kuiva', 'Normaali', 'Kostea', 'Märkä'],
    lie: ['Tiiltä', 'Väylä', 'Karheikko', 'Hiekka'],
    elevationDirection: ['Alamäki', 'Tasainen', 'Ylämäki'],
    elevationAmount: ['Pieni', 'Selvä', 'Jyrkkä'],
    windDirection: ['Tyyni', 'Vastainen', 'Myötäinen', 'Sivu vasemmalta', 'Sivu oikealta'],
    windStrength: ['Kevyt', 'Kohtalainen', 'Kova']
  };

  const toggleMenu = (key) => {
    activeMenu = activeMenu === key ? null : key;
  };

  const selectOption = (key, value) => {
    selections = { ...selections, [key]: value };
    if (!['wind', 'elevation'].includes(activeMenu)) {
      activeMenu = null;
    }
  };

  const closeMenu = () => {
    activeMenu = null;
  };

  const isActive = (key) => {
    if (key === 'wind') {
      return (
        selections.windDirection !== defaults.windDirection ||
        selections.windStrength !== defaults.windStrength
      );
    }
    if (key === 'elevation') {
      return (
        selections.elevationDirection !== defaults.elevationDirection ||
        selections.elevationAmount !== defaults.elevationAmount
      );
    }
    return selections[key] !== defaults[key];
  };

  const handleCalculate = () => {
    const value = Number(distance);

    if (!distance || Number.isNaN(value)) {
      distanceError = 'Syötä matka metreinä (10–400).';
      return;
    }

    if (value < 10 || value > 400) {
      distanceError = 'Matkan tulee olla 10–400 m.';
      return;
    }

    distanceError = '';
  };
</script>

<div
  class="min-h-screen bg-white text-zinc-900 [background:radial-gradient(1200px_500px_at_20%_-10%,rgba(190,242,100,0.22),transparent),radial-gradient(900px_500px_at_100%_0%,rgba(24,24,27,0.06),transparent)]"
>
  <div class="mx-auto flex w-full max-w-4xl flex-col gap-10 px-5 pb-20 pt-10 font-['Manrope'] sm:pt-16">
    <header class="flex flex-wrap items-center justify-between gap-4">
      <div class="space-y-1">
        <p class="text-xs uppercase tracking-[0.2em] text-zinc-500">PocketCaddie</p>
        <h1 class="font-['Fraunces'] text-2xl font-medium">Lyöntihetki</h1>
      </div>
      <button
        type="button"
        class="group relative overflow-hidden rounded-2xl bg-lime-300 px-5 py-2.5 text-sm font-medium text-zinc-900 shadow-lg shadow-lime-500/35 transition hover:-translate-y-0.5 hover:bg-lime-200 hover:shadow-lime-400/50 active:translate-y-0 active:scale-[0.99] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-lime-300"
        on:click={handleCalculate}
      >
        <span class="relative z-10">Laske oikea maila</span>
        <span
          class="absolute inset-0 -translate-x-full bg-[linear-gradient(110deg,transparent,rgba(255,255,255,0.65),transparent)] transition-transform duration-700 group-hover:translate-x-full"
        ></span>
      </button>
    </header>

    <main class="flex flex-col items-center gap-10">
      <section class="flex w-full flex-col items-center gap-4">
        <div
          class="flex h-64 w-64 items-center justify-center rounded-full bg-[conic-gradient(from_120deg,rgba(190,242,100,0.8),rgba(134,239,172,0.5),rgba(59,130,246,0.0),rgba(190,242,100,0.6))] p-2 shadow-[0_40px_90px_-55px_rgba(24,24,27,0.6)] sm:h-80 sm:w-80"
        >
          <div
            class="flex h-full w-full flex-col items-center justify-center gap-5 rounded-full border border-zinc-200/80 bg-white/95 text-center shadow-inner"
          >
            <p class="tracking-[0.25em] text-zinc-600">Lyöntimatka</p>
            <div class="flex items-baseline gap-2">
              <input
                type="number"
                min="10"
                max="400"
                inputmode="numeric"
                placeholder="150"
                class="w-24 border-none bg-transparent text-center text-4xl font-medium text-zinc-900 outline-none placeholder:text-zinc-300"
                bind:value={distance}
                on:input={() => {
                  if (distanceError) distanceError = '';
                }}
              />
              <span class="text-sm text-zinc-400">m</span>
            </div>
            <p class="text-xs text-zinc-400">Syötä matka 10–400 metriä</p>
          </div>
        </div>
        {#if distanceError}
          <p class="rounded-full border border-rose-200 bg-rose-100 px-3 py-1 text-xs font-medium text-rose-700">
            {distanceError}
          </p>
        {/if}
      </section>

      <section class="w-full">
        <div class="grid grid-cols-5 gap-2 sm:gap-3">
          <div class="relative">
            <button
              class={`flex w-full flex-col items-center gap-1 rounded-2xl border px-2 py-3 text-center shadow-[0_18px_45px_-38px_rgba(24,24,27,0.35)] transition hover:-translate-y-0.5 ${
                isActive('temp')
                  ? 'border-lime-300 bg-lime-100/70 text-zinc-900'
                  : 'border-zinc-200/80 bg-white/90 text-zinc-700'
              }`}
              on:click={() => toggleMenu('temp')}
              type="button"
            >
              <span class="text-lg">🌡️</span>
              <span class="text-[10px] lowercase tracking-wide">lämpö</span>
            </button>
          </div>

          <div class="relative">
            <button
              class={`flex w-full flex-col items-center gap-1 rounded-2xl border px-2 py-3 text-center shadow-[0_18px_45px_-38px_rgba(24,24,27,0.35)] transition hover:-translate-y-0.5 ${
                isActive('moisture')
                  ? 'border-lime-300 bg-lime-100/70 text-zinc-900'
                  : 'border-zinc-200/80 bg-white/90 text-zinc-700'
              }`}
              on:click={() => toggleMenu('moisture')}
              type="button"
            >
              <span class="text-lg">💧</span>
              <span class="text-[10px] lowercase tracking-wide">kosteus</span>
            </button>
          </div>

          <div class="relative">
            <button
              class={`flex w-full flex-col items-center gap-1 rounded-2xl border px-2 py-3 text-center shadow-[0_18px_45px_-38px_rgba(24,24,27,0.35)] transition hover:-translate-y-0.5 ${
                isActive('lie')
                  ? 'border-lime-300 bg-lime-100/70 text-zinc-900'
                  : 'border-zinc-200/80 bg-white/90 text-zinc-700'
              }`}
              on:click={() => toggleMenu('lie')}
              type="button"
            >
              <span class="text-lg">🌿</span>
              <span class="text-[10px] lowercase tracking-wide">lie</span>
            </button>
          </div>

          <div class="relative">
            <button
              class={`flex w-full flex-col items-center gap-1 rounded-2xl border px-2 py-3 text-center shadow-[0_18px_45px_-38px_rgba(24,24,27,0.35)] transition hover:-translate-y-0.5 ${
                isActive('elevation')
                  ? 'border-lime-300 bg-lime-100/70 text-zinc-900'
                  : 'border-zinc-200/80 bg-white/90 text-zinc-700'
              }`}
              on:click={() => toggleMenu('elevation')}
              type="button"
            >
              <span class="text-lg">⛰️</span>
              <span class="text-[10px] lowercase tracking-wide">korkeus</span>
            </button>
          </div>

          <div class="relative">
            <button
              class={`flex w-full flex-col items-center gap-1 rounded-2xl border px-2 py-3 text-center shadow-[0_18px_45px_-38px_rgba(24,24,27,0.35)] transition hover:-translate-y-0.5 ${
                isActive('wind')
                  ? 'border-lime-300 bg-lime-100/70 text-zinc-900'
                  : 'border-zinc-200/80 bg-white/90 text-zinc-700'
              }`}
              on:click={() => toggleMenu('wind')}
              type="button"
            >
              <span class="text-lg">💨</span>
              <span class="text-[10px] lowercase tracking-wide">tuuli</span>
            </button>
          </div>
        </div>
      </section>
    </main>
  </div>
</div>

{#if activeMenu}
  <div class="fixed inset-0 z-40 flex items-center justify-center bg-zinc-900/20 px-4 backdrop-blur-sm">
    <button class="absolute inset-0 h-full w-full" on:click={closeMenu} type="button" aria-label="Close menu"></button>
    <div
      class="relative z-10 w-full max-w-sm rounded-3xl border border-zinc-200/80 bg-white/95 p-6 shadow-[0_40px_90px_-50px_rgba(24,24,27,0.6)]"
    >
      <button
        type="button"
        class="absolute right-4 top-4 rounded-full border border-zinc-200 bg-white px-2 py-1 text-xs text-zinc-500 hover:text-zinc-900"
        on:click={closeMenu}
      >
        Sulje
      </button>

      {#if activeMenu === 'temp'}
        <div class="space-y-4">
          <div>
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Lämpötila</p>
            <h2 class="mt-1 text-xl font-medium text-zinc-900">Valitse lämpö</h2>
          </div>
          <div class="grid grid-cols-2 gap-2">
            {#each options.temp as option}
              <button
                type="button"
                class={`rounded-2xl border px-3 py-2 text-sm transition ${
                  selections.temp === option
                    ? 'border-lime-300 bg-lime-100 text-zinc-900'
                    : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('temp', option)}
              >
                {option}
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'moisture'}
        <div class="space-y-4">
          <div>
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Kosteus</p>
            <h2 class="mt-1 text-xl font-medium text-zinc-900">Valitse kentän pinta</h2>
          </div>
          <div class="grid grid-cols-2 gap-2">
            {#each options.moisture as option}
              <button
                type="button"
                class={`rounded-2xl border px-3 py-2 text-sm transition ${
                  selections.moisture === option
                    ? 'border-lime-300 bg-lime-100 text-zinc-900'
                    : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('moisture', option)}
              >
                {option}
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'lie'}
        <div class="space-y-4">
          <div>
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Lie</p>
            <h2 class="mt-1 text-xl font-medium text-zinc-900">Valitse alusta</h2>
          </div>
          <div class="grid grid-cols-2 gap-2">
            {#each options.lie as option}
              <button
                type="button"
                class={`rounded-2xl border px-3 py-2 text-sm transition ${
                  selections.lie === option
                    ? 'border-lime-300 bg-lime-100 text-zinc-900'
                    : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('lie', option)}
              >
                {option}
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'elevation'}
        <div class="space-y-5">
          <div>
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Korkeus</p>
            <h2 class="mt-1 text-xl font-medium text-zinc-900">Määritä korkeusero</h2>
          </div>
          <div class="space-y-2">
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Suunta</p>
            <div class="grid grid-cols-2 gap-2">
              {#each options.elevationDirection as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm transition ${
                    selections.elevationDirection === option
                      ? 'border-lime-300 bg-lime-100 text-zinc-900'
                      : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('elevationDirection', option)}
                >
                  {option}
                </button>
              {/each}
            </div>
          </div>
          <div class="space-y-2">
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Voimakkuus</p>
            <div class="grid grid-cols-3 gap-2">
              {#each options.elevationAmount as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm transition ${
                    selections.elevationAmount === option
                      ? 'border-lime-300 bg-lime-100 text-zinc-900'
                      : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('elevationAmount', option)}
                >
                  {option}
                </button>
              {/each}
            </div>
          </div>
          <button
            type="button"
            class="w-full rounded-2xl bg-zinc-900 py-2 text-sm font-medium text-white shadow-lg shadow-zinc-900/15"
            on:click={closeMenu}
          >
            Valmis
          </button>
        </div>
      {/if}

      {#if activeMenu === 'wind'}
        <div class="space-y-5">
          <div>
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Tuuli</p>
            <h2 class="mt-1 text-xl font-medium text-zinc-900">Määritä tuuli</h2>
          </div>
          <div class="space-y-2">
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Suunta</p>
            <div class="grid grid-cols-2 gap-2">
              {#each options.windDirection as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm transition ${
                    selections.windDirection === option
                      ? 'border-lime-300 bg-lime-100 text-zinc-900'
                      : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('windDirection', option)}
                >
                  {option}
                </button>
              {/each}
            </div>
          </div>
          <div class="space-y-2">
            <p class="text-xs uppercase tracking-[0.2em] text-zinc-400">Voimakkuus</p>
            <div class="grid grid-cols-3 gap-2">
              {#each options.windStrength as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm transition ${
                    selections.windStrength === option
                      ? 'border-lime-300 bg-lime-100 text-zinc-900'
                      : 'border-zinc-200 text-zinc-600 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('windStrength', option)}
                >
                  {option}
                </button>
              {/each}
            </div>
          </div>
          <button
            type="button"
            class="w-full rounded-2xl bg-zinc-900 py-2 text-sm font-medium text-white shadow-lg shadow-zinc-900/15"
            on:click={closeMenu}
          >
            Valmis
          </button>
        </div>
      {/if}
    </div>
  </div>
{/if}
