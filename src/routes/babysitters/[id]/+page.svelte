<script>
  import { page } from '$app/stores';
  import { onMount } from 'svelte';

  let babysitter = null;
  let loading = true;
  const { id } = $page.params;

  onMount(() => {
    fetch("https://raw.githubusercontent.com/AliAhmed205/InGoodHands/refs/heads/main/src/Database/about-babysitters.json")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Netwerk response was niet ok: " + response.statusText);
        }
        return response.json();
      })
      .then((babysitters) => {
        babysitter = babysitters.find(b => String(b.id) === String(id));

        if (!babysitter) {
          throw new Error('Babysitter niet gevonden');
        }

        loading = false;
      })
      .catch((error) => {
        console.error("Er was een probleem met het ophalen van de babysitter data:", error);
        loading = false;
      });
  });
</script>

{#if loading}
  <p>Loading...</p>
{:else if babysitter}
  <section>
    <h1>{babysitter.naam}</h1>
    <img src={babysitter.profielFoto} alt={`Foto van ${babysitter.naam}`} />
    <p>Locatie: {babysitter.locatie}</p>
    <p>Gemiddelde review score: {babysitter.gemiddeldeReviewScore}</p>
    <p>{babysitter.bio}</p>
    <a href="/babysitters">Terug naar de lijst</a>
  </section>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}
