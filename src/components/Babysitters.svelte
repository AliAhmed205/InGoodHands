<script>
  import { onMount } from "svelte";
  let babysittersList = [];

  onMount(async () => {
    fetch("./src/Database/about-babysitters.json")
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok" + response.statusText);
        }
        return response.json();
      })

      .then((data) => {
        babysittersList = data;
      })
      .catch((error) => {
        console.error(
          "Er was een probleem met het ophalen van de data:",
          error
        );
      });
  });
</script>

<section>
  {#if babysittersList.length > 0}
    <ul class="about-us">
      {#each babysittersList as babysitter}
        <li>
          <a href="#"><img src={babysitter.profielFoto} alt="Foto van {babysitter.naam}" srcset=""> {babysitter.naam}</a>
        </li>
      {/each}
    </ul>
  {:else}
    <p>Geen babysitters gevonden</p>
  {/if}
</section>
