<script>
  import { onMount } from "svelte";
  let babysittersList = [];

  onMount(async () => {
    fetch("https://raw.githubusercontent.com/AliAhmed205/InGoodHands/refs/heads/main/src/Database/about-babysitters.json")
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

<section class="babysitter-container" id="the-babysitters">
  <h2>Meet the babysitters</h2>
  <p>
    This is our fantastic team of babysitters. Each and every one of them own
    their own specfic type of skills, so they can offer you the best experience.
  </p>
  <div class="babysitter-list">
    {#if babysittersList.length > 0}
      <ul class="about-us">
        {#each babysittersList as babysitter}
          <li>
            <a href={`/babysitters/${babysitter.id}`}>
              <img
                src={babysitter.profielFoto}
                alt="Foto van {babysitter.naam}"
                srcset=""
              />
              <h4>{babysitter.naam}</h4>
              <p>{babysitter.locatie}</p>
              <p>
                {babysitter.gemiddeldeReviewScore}
                <i class="fa-solid fa-star"></i> gemiddeld
              </p>
            </a>
          </li>
        {/each}
      </ul>
    {:else}
      <p>Geen babysitters gevonden</p>
    {/if}
  </div>
</section>
