<svelte:head>
  <title>PocketCaddie — Tulossa pian</title>
  <meta name="description" content="PocketCaddie antaa jokaiselle lyönnille nopean mailasuosituksen ja auttaa tunnistamaan yleisimmät missit." />
  <meta property="og:title" content="PocketCaddie — Tulossa pian" />
  <meta property="og:description" content="PocketCaddie antaa jokaiselle lyönnille nopean mailasuosituksen ja auttaa tunnistamaan yleisimmät missit." />
  <meta property="og:type" content="website" />
</svelte:head>

<script>
  import TallySignupEmbed from '$lib/components/landingpage/TallySignupEmbed.svelte';
  import EmailSignupForm from '$lib/components/landingpage/EmailSignupForm.svelte';
  import ScreenshotGallery from '$lib/components/landingpage/ScreenshotGallery.svelte';
  import FeatureCards from '$lib/components/landingpage/FeatureCards.svelte';

  const SIGNUP_VARIANT = 'tally'; // 'tally' or 'local'
</script>

<main class="mx-auto w-full max-w-5xl space-y-10 px-5 pb-20 pt-12 font-['Manrope'] sm:pt-16 sm:space-y-12">
      <section class="space-y-6">
        <div class="space-y-4">
          <h1 class="max-w-xl font-['Fraunces'] text-4xl tracking-tight sm:text-5xl">
            Lopeta mailan arpominen.
          </h1>
          <p class="max-w-2xl text-base leading-relaxed text-zinc-600 sm:text-lg">
            PocketCaddie antaa jokaiselle lyönnille nopean mailasuosituksen ja auttaa tunnistamaan yleisimmät missit.
          </p>
          <p class="max-w-2xl leading-relaxed text-zinc-600 sm:text-md">
            Jos kiinnostuit, voit ilmoittautua odotuslistalle, niin saat tiedon heti kun sovellus on saatavilla!
          </p>
        </div>

        {#if SIGNUP_VARIANT === 'tally'}
          <TallySignupEmbed />
        {:else}
          <EmailSignupForm />
        {/if}
      </section>

      <ScreenshotGallery />

      <FeatureCards />
</main>
