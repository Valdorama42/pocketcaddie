<svelte:head>
  <title>PockerCaddie — Lyönti</title>
</svelte:head>

<script>
  import { spring } from 'svelte/motion';
  const minDistance = 10;
  const maxDistance = 400;

  let distance = 150;
  let error = '';
  let activeMenu = null;
  let closeTimer;

  const defaults = {
    temp: 'Normal (10–25°C)',
    windDirection: 'Calm',
    windStrength: 'Light',
    humid: 'Normal',
    elevDirection: 'Flat',
    elevAmount: 'Slight',
    dir: 'Straight'
  };

  let selections = { ...defaults };

  const options = {
    temp: ['Cold (<10°C)', 'Normal (10–25°C)', 'Hot (>25°C)'],
    humid: ['Dry', 'Normal', 'Wet'],
    dir: ['Straight', 'Left', 'Right'],
    windDirection: ['Calm', 'Headwind', 'Tailwind', 'Crosswind L', 'Crosswind R'],
    windStrength: ['Light', 'Medium', 'Strong'],
    elevDirection: ['Downhill', 'Flat', 'Uphill'],
    elevAmount: ['Slight', 'Moderate', 'Steep']
  };

  const sheetY = spring(100, { stiffness: 0.12, damping: 0.35 });

  const clamp = (value) => Math.min(maxDistance, Math.max(minDistance, value));

  const adjustDistance = (delta) => {
    distance = clamp(distance + delta);
    error = '';
  };

  const handleDistanceInput = (event) => {
    const value = Number(event.target.value);
    if (Number.isNaN(value)) {
      error = 'Syötä matka metreinä (10–400).';
      return;
    }
    distance = value;
    if (value < minDistance || value > maxDistance) {
      error = 'Matkan tulee olla 10–400 m.';
      return;
    }
    error = '';
  };

  const openMenu = (key) => {
    clearTimeout(closeTimer);
    activeMenu = key;
    sheetY.set(0);
  };

  const closeMenu = () => {
    sheetY.set(100);
    clearTimeout(closeTimer);
    closeTimer = setTimeout(() => {
      activeMenu = null;
    }, 220);
  };

  const toggleMenu = (key) => {
    if (activeMenu === key) {
      closeMenu();
      return;
    }
    openMenu(key);
  };

  const selectOption = (key, value) => {
    selections = { ...selections, [key]: value };
    if (!['wind', 'elev'].includes(activeMenu)) {
      closeMenu();
    }
  };

  const isActive = (key) => {
    if (key === 'wind') {
      return (
        selections.windDirection !== defaults.windDirection ||
        selections.windStrength !== defaults.windStrength
      );
    }
    if (key === 'elev') {
      return (
        selections.elevDirection !== defaults.elevDirection ||
        selections.elevAmount !== defaults.elevAmount
      );
    }
    return selections[key] !== defaults[key];
  };

  const saveShot = () => {
    if (!distance || distance < minDistance || distance > maxDistance) {
      error = 'Matkan tulee olla 10–400 m.';
      return;
    }
    error = '';
  };
</script>

<main class="mx-auto flex w-full max-w-md flex-col gap-10 px-6 pb-14 pt-12 font-['Manrope']">
    <header class="flex items-start justify-between">
      <div>
        <p class="text-sm text-zinc-500">Hole 7 • Par 4</p>
        <h1 class="mt-1 text-3xl font-semibold text-zinc-900">Shot Tracker</h1>
      </div>
      <div
        class="flex h-12 w-12 items-center justify-center rounded-full bg-[#c7f200] text-lg font-semibold text-zinc-900 shadow-[0_10px_30px_-18px_rgba(132,204,22,0.8)]"
      >
        4
      </div>
    </header>

    <section class="relative flex flex-col items-center gap-4">
      <button
        type="button"
        class="absolute left-0 top-1/2 -translate-x-1/2 -translate-y-1/2 rounded-full bg-white px-4 py-3 text-xl text-zinc-700 shadow-[0_10px_30px_-18px_rgba(15,23,42,0.4)]"
        on:click={() => adjustDistance(-1)}
        aria-label="Vähennä matkaa"
      >
        –
      </button>
      <button
        type="button"
        class="absolute right-0 top-1/2 translate-x-1/2 -translate-y-1/2 rounded-full bg-white px-4 py-3 text-xl text-zinc-700 shadow-[0_10px_30px_-18px_rgba(15,23,42,0.4)]"
        on:click={() => adjustDistance(1)}
        aria-label="Lisää matkaa"
      >
        +
      </button>

      <div class="relative flex h-72 w-72 items-center justify-center">
        <div class="absolute inset-0 rounded-full border border-zinc-200/60"></div>
        <div class="absolute inset-4 rounded-full border border-zinc-200/50"></div>
        <div class="absolute inset-8 rounded-full border border-zinc-200/40"></div>
        <div class="absolute inset-12 rounded-full border border-zinc-200/30"></div>
        <div
          class="absolute inset-6 rounded-full border-16 border-[#c7f200] bg-white shadow-[0_20px_60px_-40px_rgba(132,204,22,0.65)]"
        ></div>
        <div class="relative flex flex-col items-center">
          <div class="relative">
            <input
              type="number"
              min={minDistance}
              max={maxDistance}
              class="w-28 border-none bg-transparent text-center text-6xl font-semibold text-zinc-900 outline-none"
              value={distance}
              on:input={handleDistanceInput}
            />
          </div>
          <p class="mt-2 text-sm text-zinc-500">meters</p>
        </div>
      </div>

      {#if error}
        <p class="rounded-full border border-rose-200 bg-rose-100 px-3 py-1 text-xs font-medium text-rose-700">
          {error}
        </p>
      {/if}
    </section>

    <section class="space-y-4">
      <p class="text-sm font-semibold text-zinc-500">Environment Settings</p>
      <div class="grid grid-cols-5 gap-3">
        <button
          type="button"
          class={`group flex flex-col items-center gap-2 rounded-2xl bg-white px-3 py-4 text-[11px] font-semibold text-zinc-500 shadow-[0_10px_24px_-18px_rgba(15,23,42,0.25)] transition ${
            isActive('temp') ? 'ring-2 ring-[#c7f200] text-zinc-900' : ''
          }`}
          on:click={() => toggleMenu('temp')}
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full bg-zinc-50 text-zinc-500">
            <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.6">
              <path d="M12 4a3 3 0 0 0-3 3v7.2a4 4 0 1 0 6 0V7a3 3 0 0 0-3-3Z" />
              <path d="M12 14.5V7" />
            </svg>
          </span>
          Temp
        </button>
        <button
          type="button"
          class={`group flex flex-col items-center gap-2 rounded-2xl bg-white px-3 py-4 text-[11px] font-semibold text-zinc-500 shadow-[0_10px_24px_-18px_rgba(15,23,42,0.25)] transition ${
            isActive('wind') ? 'ring-2 ring-[#c7f200] text-zinc-900' : ''
          }`}
          on:click={() => toggleMenu('wind')}
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full bg-zinc-50 text-zinc-500">
            <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.6">
              <path d="M3 8h10a3 3 0 1 0-3-3" />
              <path d="M3 12h14a3 3 0 1 1-3 3" />
              <path d="M3 16h8" />
            </svg>
          </span>
          Wind
        </button>
        <button
          type="button"
          class={`group flex flex-col items-center gap-2 rounded-2xl bg-white px-3 py-4 text-[11px] font-semibold text-zinc-500 shadow-[0_10px_24px_-18px_rgba(15,23,42,0.25)] transition ${
            isActive('humid') ? 'ring-2 ring-[#c7f200] text-zinc-900' : ''
          }`}
          on:click={() => toggleMenu('humid')}
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full bg-zinc-50 text-zinc-500">
            <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.6">
              <path d="M12 3c3.5 4 6 7 6 10a6 6 0 0 1-12 0c0-3 2.5-6 6-10Z" />
            </svg>
          </span>
          Humid
        </button>
        <button
          type="button"
          class={`group flex flex-col items-center gap-2 rounded-2xl bg-white px-3 py-4 text-[11px] font-semibold text-zinc-500 shadow-[0_10px_24px_-18px_rgba(15,23,42,0.25)] transition ${
            isActive('elev') ? 'ring-2 ring-[#c7f200] text-zinc-900' : ''
          }`}
          on:click={() => toggleMenu('elev')}
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full bg-zinc-50 text-zinc-500">
            <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.6">
              <path d="m3 17 6-8 4 6 3-4 5 6" />
              <path d="M3 17h18" />
            </svg>
          </span>
          Elev
        </button>
        <button
          type="button"
          class={`group flex flex-col items-center gap-2 rounded-2xl bg-white px-3 py-4 text-[11px] font-semibold text-zinc-500 shadow-[0_10px_24px_-18px_rgba(15,23,42,0.25)] transition ${
            isActive('dir') ? 'ring-2 ring-[#c7f200] text-zinc-900' : ''
          }`}
          on:click={() => toggleMenu('dir')}
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full bg-zinc-50 text-zinc-500">
            <svg viewBox="0 0 24 24" class="h-5 w-5" fill="none" stroke="currentColor" stroke-width="1.6">
              <circle cx="12" cy="12" r="8" />
              <path d="M12 7l2.5 5-5 2.5Z" />
            </svg>
          </span>
          Dir
        </button>
      </div>
    </section>

    <button
      type="button"
      class="w-full rounded-2xl bg-[#c7f200] py-4 text-base font-semibold text-zinc-900 shadow-[0_18px_40px_-25px_rgba(132,204,22,0.75)] transition hover:-translate-y-0.5 hover:bg-[#d3ff1a]"
      on:click={saveShot}
    >
      Save Shot
    </button>
</main>

{#if activeMenu}
  <div class="fixed inset-0 z-40 flex items-end justify-center bg-black/10 px-4 pb-6 backdrop-blur-sm">
    <button class="absolute inset-0 h-full w-full" on:click={closeMenu} type="button" aria-label="Close"></button>
    <div
      class="relative z-10 w-full max-w-md rounded-[28px] border border-zinc-200/80 bg-white/95 p-5 shadow-[0_40px_90px_-50px_rgba(24,24,27,0.5)]"
      style={`transform: translateY(${$sheetY}%);`}
    >
      <div class="mx-auto mb-4 h-1.5 w-12 rounded-full bg-zinc-200"></div>

      {#if activeMenu === 'temp'}
        <div class="space-y-4">
          <div class="flex items-center justify-between rounded-2xl bg-rose-50 px-4 py-3">
            <div class="flex items-center gap-3">
              <span class="flex h-11 w-11 items-center justify-center rounded-full bg-rose-100 text-rose-500">
                <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.6">
                  <path d="M12 4a3 3 0 0 0-3 3v7.2a4 4 0 1 0 6 0V7a3 3 0 0 0-3-3Z" />
                  <path d="M12 14.5V7" />
                </svg>
              </span>
              <p class="text-lg font-semibold text-zinc-900">Temperature</p>
            </div>
            <button
              type="button"
              class="flex h-9 w-9 items-center justify-center rounded-full bg-white text-zinc-500 shadow-sm"
              on:click={closeMenu}
              aria-label="Close"
            >
              ×
            </button>
          </div>
          <div class="space-y-3">
            {#each options.temp as option}
              <button
                type="button"
                class={`flex w-full items-center gap-3 rounded-2xl border px-4 py-3 text-left text-sm font-medium transition ${
                  selections.temp === option
                    ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                    : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('temp', option)}
              >
                <span class="text-xl">
                  {option.startsWith('Cold') ? '❄️' : option.startsWith('Normal') ? '☀️' : '🔥'}
                </span>
                <span>{option}</span>
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'humid'}
        <div class="space-y-4">
          <div class="flex items-center justify-between rounded-2xl bg-sky-50 px-4 py-3">
            <div class="flex items-center gap-3">
              <span class="flex h-11 w-11 items-center justify-center rounded-full bg-sky-100 text-sky-500">
                <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.6">
                  <path d="M12 3c3.5 4 6 7 6 10a6 6 0 0 1-12 0c0-3 2.5-6 6-10Z" />
                </svg>
              </span>
              <p class="text-lg font-semibold text-zinc-900">Humidity</p>
            </div>
            <button
              type="button"
              class="flex h-9 w-9 items-center justify-center rounded-full bg-white text-zinc-500 shadow-sm"
              on:click={closeMenu}
              aria-label="Close"
            >
              ×
            </button>
          </div>
          <div class="space-y-3">
            {#each options.humid as option}
              <button
                type="button"
                class={`flex w-full items-center gap-3 rounded-2xl border px-4 py-3 text-left text-sm font-medium transition ${
                  selections.humid === option
                    ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                    : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('humid', option)}
              >
                <span class="text-xl">
                  {option === 'Dry' ? '🏜️' : option === 'Normal' ? '💧' : '🌧️'}
                </span>
                <span>{option}</span>
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'dir'}
        <div class="space-y-4">
          <div class="flex items-center justify-between rounded-2xl bg-amber-50 px-4 py-3">
            <div class="flex items-center gap-3">
              <span class="flex h-11 w-11 items-center justify-center rounded-full bg-amber-100 text-amber-500">
                <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.6">
                  <circle cx="12" cy="12" r="8" />
                  <path d="M12 7l2.5 5-5 2.5Z" />
                </svg>
              </span>
              <p class="text-lg font-semibold text-zinc-900">Direction</p>
            </div>
            <button
              type="button"
              class="flex h-9 w-9 items-center justify-center rounded-full bg-white text-zinc-500 shadow-sm"
              on:click={closeMenu}
              aria-label="Close"
            >
              ×
            </button>
          </div>
          <div class="space-y-3">
            {#each options.dir as option}
              <button
                type="button"
                class={`flex w-full items-center gap-3 rounded-2xl border px-4 py-3 text-left text-sm font-medium transition ${
                  selections.dir === option
                    ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                    : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                }`}
                on:click={() => selectOption('dir', option)}
              >
                <span class="text-xl">
                  {option === 'Left' ? '⬅️' : option === 'Right' ? '➡️' : '⬆️'}
                </span>
                <span>{option}</span>
              </button>
            {/each}
          </div>
        </div>
      {/if}

      {#if activeMenu === 'wind'}
        <div class="space-y-5">
          <div class="flex items-center justify-between rounded-2xl bg-sky-50 px-4 py-3">
            <div class="flex items-center gap-3">
              <span class="flex h-11 w-11 items-center justify-center rounded-full bg-sky-100 text-sky-500">
                <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.6">
                  <path d="M3 8h10a3 3 0 1 0-3-3" />
                  <path d="M3 12h14a3 3 0 1 1-3 3" />
                  <path d="M3 16h8" />
                </svg>
              </span>
              <p class="text-lg font-semibold text-zinc-900">Wind</p>
            </div>
            <button
              type="button"
              class="flex h-9 w-9 items-center justify-center rounded-full bg-white text-zinc-500 shadow-sm"
              on:click={closeMenu}
              aria-label="Close"
            >
              ×
            </button>
          </div>
          <div class="space-y-2">
            <p class="text-xs font-semibold uppercase tracking-[0.2em] text-zinc-400">Direction</p>
            <div class="space-y-3">
              {#each options.windDirection as option}
                <button
                  type="button"
                  class={`flex w-full items-center gap-3 rounded-2xl border px-4 py-3 text-left text-sm font-medium transition ${
                    selections.windDirection === option
                      ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                      : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('windDirection', option)}
                >
                  <span class="text-xl">💨</span>
                  <span>{option}</span>
                </button>
              {/each}
            </div>
          </div>
          <div class="space-y-2">
            <p class="text-xs font-semibold uppercase tracking-[0.2em] text-zinc-400">Strength</p>
            <div class="grid grid-cols-3 gap-2">
              {#each options.windStrength as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm font-medium transition ${
                    selections.windStrength === option
                      ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                      : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
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
            class="w-full rounded-2xl bg-zinc-900 py-2 text-sm font-medium text-white shadow-lg shadow-zinc-900/10"
            on:click={closeMenu}
          >
            Done
          </button>
        </div>
      {/if}

      {#if activeMenu === 'elev'}
        <div class="space-y-5">
          <div class="flex items-center justify-between rounded-2xl bg-violet-50 px-4 py-3">
            <div class="flex items-center gap-3">
              <span class="flex h-11 w-11 items-center justify-center rounded-full bg-violet-100 text-violet-500">
                <svg viewBox="0 0 24 24" class="h-6 w-6" fill="none" stroke="currentColor" stroke-width="1.6">
                  <path d="m3 17 6-8 4 6 3-4 5 6" />
                  <path d="M3 17h18" />
                </svg>
              </span>
              <p class="text-lg font-semibold text-zinc-900">Elevation</p>
            </div>
            <button
              type="button"
              class="flex h-9 w-9 items-center justify-center rounded-full bg-white text-zinc-500 shadow-sm"
              on:click={closeMenu}
              aria-label="Close"
            >
              ×
            </button>
          </div>
          <div class="space-y-2">
            <p class="text-xs font-semibold uppercase tracking-[0.2em] text-zinc-400">Slope</p>
            <div class="space-y-3">
              {#each options.elevDirection as option}
                <button
                  type="button"
                  class={`flex w-full items-center gap-3 rounded-2xl border px-4 py-3 text-left text-sm font-medium transition ${
                    selections.elevDirection === option
                      ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                      : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('elevDirection', option)}
                >
                  <span class="text-xl">
                    {option === 'Downhill' ? '⬇️' : option === 'Uphill' ? '⬆️' : '➡️'}
                  </span>
                  <span>{option}</span>
                </button>
              {/each}
            </div>
          </div>
          <div class="space-y-2">
            <p class="text-xs font-semibold uppercase tracking-[0.2em] text-zinc-400">Intensity</p>
            <div class="grid grid-cols-3 gap-2">
              {#each options.elevAmount as option}
                <button
                  type="button"
                  class={`rounded-2xl border px-3 py-2 text-sm font-medium transition ${
                    selections.elevAmount === option
                      ? 'border-[#c7f200] bg-[#f6ffe0] text-zinc-900'
                      : 'border-zinc-100 bg-white text-zinc-700 hover:bg-zinc-50'
                  }`}
                  on:click={() => selectOption('elevAmount', option)}
                >
                  {option}
                </button>
              {/each}
            </div>
          </div>
          <button
            type="button"
            class="w-full rounded-2xl bg-zinc-900 py-2 text-sm font-medium text-white shadow-lg shadow-zinc-900/10"
            on:click={closeMenu}
          >
            Done
          </button>
        </div>
      {/if}
    </div>
  </div>
{/if}
