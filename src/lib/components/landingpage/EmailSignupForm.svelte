<script>
  let email = '';
  let feedback = '';
  let success = false;

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

<div class="rounded-[36px] bg-white p-6 shadow-[0_24px_60px_-38px_rgba(24,24,27,0.22)] sm:p-7">
  <form class="flex flex-col gap-4" on:submit|preventDefault={handleSubmit}>
    <label class="sr-only" for="email">Sähköposti</label>
    <input
      id="email"
      type="email"
      placeholder="Sähköposti"
      class="w-full rounded-2xl border border-zinc-200 bg-white px-5 py-4 text-base text-zinc-900 shadow-sm outline-none transition placeholder:text-zinc-400 focus:border-lime-300 focus:ring-2 focus:ring-lime-200"
      bind:value={email}
      autocomplete="email"
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
          success ? 'border-lime-200 bg-lime-100 text-lime-900' : 'border-rose-200 bg-rose-100 text-rose-700'
        }`}
        aria-live="polite"
      >
        {feedback}
      </div>
    {/if}
  </div>
</div>
