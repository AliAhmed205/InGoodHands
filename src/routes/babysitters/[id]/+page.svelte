<script>
  import { page } from "$app/stores";
  import { onMount, onDestroy } from "svelte";
  import Header from "../../../components/Header.svelte"
  import cloudImage from '../../../images/cloud.png';
  import PinImage from "../../../images/Drawing-Pin.png"

  import "../[id]/detail.css"


  let babysitter = null;
  let loading = true;
  const { id } = $page.params;

  onMount(async () => {
    try {
      const response = await fetch(
        "https://raw.githubusercontent.com/AliAhmed205/InGoodHands/refs/heads/main/src/Database/about-babysitters.json"
      );

      if (!response.ok) {
        throw new Error(
          "Netwerk response was niet ok: " + response.statusText
        );
      }

      const babysitters = await response.json();
      babysitter = babysitters.find((b) => String(b.id) === String(id));

      if (!babysitter) {
        throw new Error("Babysitter niet gevonden");
      }
    } catch (error) {
      console.error(
        "Er was een probleem met het ophalen van de babysitter data:",
        error
      );
    } finally {
      loading = false;  
    }

    const handleScroll = () => {
      const cardDetail = document.getElementById("detail-wrapper");
      if (cardDetail) {
        if (window.scrollY > 10) {
          cardDetail.classList.add("active");
        } else {
          cardDetail.classList.remove("active");
        }
      }
    };

    // window.addEventListener("scroll", handleScroll);

    // onDestroy(() => {
    //   window.removeEventListener("scroll", handleScroll);
    // });
  });
</script>



{#if loading}
  <p>Loading...</p>
{:else if babysitter}
<Header />
<main class="detail-main">
  <section class="babysitter-detail-container">
    <section class="image-container">
      <img src={PinImage} alt="Pin-item">
      <div class="text-container">
        <h1>
          {babysitter.naam}
          <i class="fa-solid fa-certificate"></i>
        </h1>
        <h3>€ {babysitter.uurtarief},00/hour</h3>
      </div>
      <img src={babysitter.profielFoto} alt={`Foto van ${babysitter.naam}`} />
    </section>

    <section class="detail-wrapper" id="detail-wrapper">
      <a href="/babysitters"> <i class="fa-solid fa-arrow-left"></i> Back to babysitters</a>
      <section class="detail one">
        <p><i class="fa-solid fa-thumbtack"></i> {babysitter.locatie}</p>
        <div class="star-container">
          {#each Array(babysitter.gemiddeldeReviewScore) as _, i}
            <i class="fa-solid fa-star" key={i}></i>
          {/each}
          <p>({babysitter.totaalAantalReviews}) Reviews</p>
        </div>
      </section>
      
      <p class="babysitter-bio">{babysitter.bio}</p>
      
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
      <div class="experience">
        <i class="fa-regular fa-address-card"></i>
        <div class="experience-text">
          <h3>Has experience with</h3>
          <ul>
            {#each babysitter.kenmerken as kenmerk}
              <li>{kenmerk}</li>
            {/each}
          </ul>
        </div>
      </div>
      
      <div class="babysitter-schedule">
        <table>
          <caption> Available </caption>
          <thead>
            <tr>
              <th scope="col"></th>
              {#each Object.keys(babysitter.beschikbaarheid) as dag}
                <th scope="col">{dag}</th>
              {/each}
            </tr>
          </thead>
          <tbody>
            <tr>
              <th scope="row">Morning</th>
              {#each Object.keys(babysitter.beschikbaarheid) as dag}
                <td>
                  {#if babysitter.beschikbaarheid[dag].morning}
                    <i class="fa-solid fa-circle"></i>
                  {:else}
                    <i class="fa-regular fa-circle"></i>
                  {/if}
                </td>
              {/each}
            </tr>
            <tr>
              <th scope="row">Noon</th>
              {#each Object.keys(babysitter.beschikbaarheid) as dag}
                <td>
                  {#if babysitter.beschikbaarheid[dag].noon}
                    <i class="fa-solid fa-circle"></i>
                  {:else}
                    <i class="fa-regular fa-circle"></i>
                  {/if}
                </td>
              {/each}
            </tr>
            <tr>
              <th scope="row">Afternoon</th>
              {#each Object.keys(babysitter.beschikbaarheid) as dag}
                <td>
                  {#if babysitter.beschikbaarheid[dag].afternoon}
                    <i class="fa-solid fa-circle"></i>
                  {:else}
                    <i class="fa-regular fa-circle"></i>
                  {/if}
                </td>
              {/each}
            </tr>
            <tr>
              <th scope="row">Evening</th>
              {#each Object.keys(babysitter.beschikbaarheid) as dag}
                <td>
                  {#if babysitter.beschikbaarheid[dag].evening}
                    <i class="fa-solid fa-circle"></i>
                  {:else}
                    <i class="fa-regular fa-circle"></i>
                  {/if}
                </td>
              {/each}
            </tr>
          </tbody>
        </table>
        <button on:click={() => window.location.href = `/babysitters/${babysitter.id}/form`}>
          <i class="fa-solid fa-baby"></i> Book {babysitter.naam} now
        </button>
      </div>
    </section>
  </section>
  <div aria-hidden="true" class="cloud-container">
    <img src={cloudImage} alt="Wolken">
    <img src={cloudImage} alt="Wolken">
    <img src={cloudImage} alt="Wolken">
  </div>
</main>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}
