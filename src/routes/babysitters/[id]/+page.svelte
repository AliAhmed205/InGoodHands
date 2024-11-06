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
      <h3>€ {babysitter.uurtarief},00/hour</h3>
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
      <hr>
      <p class="babysitter-bio">{babysitter.bio}</p>
      <hr>
      <div class="experience">
        <i class="fa-regular fa-address-card"></i>
        <div class="experience-text">
          <h3>Babysit Experience</h3>
            <p>{babysitter.ervaring} years</p>
        </div>
      </div>
      <div class="experience">
        <i class="fa-regular fa-address-card"></i>
        <div class="experience-text">
          <h3>Has experience with</h3>
          <ul>
            {#each babysitter.ervaringKind as kind}
            <li>{kind}</li>
          {/each}
          </ul>
        </div>
      </div>
      <hr>
      <a href="/babysitters">Terug naar de lijst</a>
      <div class="babysitter-schedule">
      </div>
      <table>
        <caption>
          beschikbaar
        </caption>
        <thead>
          <tr>
            <th scope="col">Person</th>
            <th scope="col">Most interest in</th>
            <th scope="col">Age</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row">Chris</th>
            <td>HTML tables</td>
            <td>22</td>
          </tr>
          <tr>
            <th scope="row">Dennis</th>
            <td>Web accessibility</td>
            <td>45</td>
          </tr>
          <tr>
            <th scope="row">Sarah</th>
            <td>JavaScript frameworks</td>
            <td>29</td>
          </tr>
          <tr>
            <th scope="row">Karen</th>
            <td>Web performance</td>
            <td>36</td>
          </tr>
        </tbody>
      </table>
      
    </section>
  </section>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}
