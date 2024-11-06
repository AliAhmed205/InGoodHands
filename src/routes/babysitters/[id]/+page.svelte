<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";

  let babysitter = null;
  let loading = true;
  const { id } = $page.params;

  onMount(() => {
    fetch(
      "https://raw.githubusercontent.com/AliAhmed205/InGoodHands/refs/heads/main/src/Database/about-babysitters.json"
    )
      .then((response) => {
        if (!response.ok) {
          throw new Error(
            "Netwerk response was niet ok: " + response.statusText
          );
        }
        return response.json();
      })
      .then((babysitters) => {
        babysitter = babysitters.find((b) => String(b.id) === String(id));

        if (!babysitter) {
          throw new Error("Babysitter niet gevonden");
        }

        loading = false;
      })
      .catch((error) => {
        console.error(
          "Er was een probleem met het ophalen van de babysitter data:",
          error
        );
        loading = false;
      });
  });
</script>

{#if loading}
  <p>Loading...</p>
{:else if babysitter}
  <section class="babysitter-detail-container">
    <section class="image-container">
      <div class="text-container">
      <h1>
        {babysitter.naam}
        <i class="fa-solid fa-certificate"></i>
      </h1>
    </div>
      <img src={babysitter.profielFoto} alt={`Foto van ${babysitter.naam}`} />
    </section>

    <section class="detail-wrapper">
      <section class="detail one">
        <p><i class="fa-solid fa-thumbtack"></i> {babysitter.locatie}</p>
        <div class="star-container">
          {#each Array(babysitter.gemiddeldeReviewScore) as _, i}
            <i class="fa-solid fa-star" key={i}></i>
          {/each}
          <p>({babysitter.gemiddeldeReviewScore}) Reviews</p>
        </div>
      </section>
      <p>{babysitter.bio}</p>
      <a href="/babysitters">Terug naar de lijst</a>
    </section>
  </section>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}
