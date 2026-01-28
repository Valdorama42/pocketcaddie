<script>
  import { onMount } from 'svelte';
  import { PUBLIC_TALLY_EMBED_URL } from '$env/static/public';

  export let embedUrl = PUBLIC_TALLY_EMBED_URL;

  const loadTallyEmbeds = () => {
    if (typeof window === 'undefined') {
      return;
    }

    if (window.Tally && typeof window.Tally.loadEmbeds === 'function') {
      window.Tally.loadEmbeds();
      return;
    }

    const scriptSrc = 'https://tally.so/widgets/embed.js';
    if (document.querySelector(`script[src="${scriptSrc}"]`)) {
      return;
    }

    const script = document.createElement('script');
    script.src = scriptSrc;
    script.onload = () => window.Tally?.loadEmbeds?.();
    script.onerror = () => window.Tally?.loadEmbeds?.();
    document.body.appendChild(script);
  };

  onMount(() => {
    loadTallyEmbeds();
  });
</script>

<div class="rounded-[36px] border border-lime-200/70 bg-white/95 p-6 shadow-[0_24px_60px_-38px_rgba(24,24,27,0.22)] ring-1 ring-lime-100/70 sm:p-7">
  {#if embedUrl}
    <div class="rounded-[28px] border border-zinc-200/80 bg-white/95 p-2 shadow-[inset_0_1px_0_rgba(255,255,255,0.8)]">
      <iframe
        data-tally-src={embedUrl}
        loading="lazy"
        width="100%"
        height="152"
        frameborder="0"
        marginheight="0"
        marginwidth="0"
        title="PocketCaddie ilmoittautumislomake"
        class="w-full rounded-[24px] bg-white"
      ></iframe>
    </div>
  {:else}
    <div class="rounded-[28px] border border-rose-200/70 bg-rose-50/70 p-4 text-sm text-rose-700">
      Aseta PUBLIC_TALLY_EMBED_URL .env-tiedostoon.
    </div>
  {/if}
  <div class="pt-4 text-xs text-zinc-500">
    <p>Lähetän vain julkaisu- ja beta-kutsut. Ei spämmiä. </p>
  </div>
</div>
