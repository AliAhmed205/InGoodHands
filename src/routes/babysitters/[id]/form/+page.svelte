<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import Header from "../../../../components/Header.svelte";

  let babysitter = null;
  let loading = true;


  let allergy = null;
  let medication = null;
  let ages = Array.from({ length: 17 }, (_, i) => i); 


  const { id } = $page.params;

  onMount(async () => {
    try {
      const response = await fetch(
        "https://raw.githubusercontent.com/AliAhmed205/InGoodHands/refs/heads/main/src/Database/about-babysitters.json"
      );

      if (!response.ok) {
        throw new Error("Netwerk response was niet ok: " + response.statusText);
      }

      const babysitters = await response.json();
      babysitter = babysitters.find((b) => String(b.id) === String(id));

      if (!babysitter) {
        throw new Error("Babysitter niet gevonden");
      }
    } catch (error) {
      console.error("Er was een probleem met het ophalen van de babysitter data:", error);
    } finally {
      loading = false;
    }
  });

</script>

{#if loading}
  <p>Loading...</p>
{:else if babysitter}
<Header />
  <h1>Boek {babysitter.naam}</h1>
  <main class="book-form">
  <form action="/">
    <div>
      <h2><i class="fa-solid fa-user"></i> Parent's Contact Information</h2>
      <label for="userName">First Name
        <input type="text" name="userName" id="userName" required />
      </label>
    
      <label for="surName">Last Name
        <input type="text" name="surName" id="surName" required />
      </label>
    
      <label for="phoneNumber">Phone Number
        <input type="number" name="phoneNumber" id="phoneNumber" />
      </label>
    
      <label for="userEmail">E-mail
        <input id="userEmail" type="email" name="userEmail" required />
      </label>
    </div>
    
    <div>
      <h2><i class="fa-solid fa-child"></i> Child's Information</h2>

      <label for="userName">First Name
        <input type="text" name="userName" id="userName" required />
      </label>

      <label for="surName">Last Name
        <input type="text" name="surName" id="surName" required />
      </label>
      
      <label for="selectAge">Age
        <select name="selectAge" id="selectAge" required>
          {#each ages as age}
            <option value={age}>{age}</option>
          {/each}
        </select>
      </label>

      <label for="allergyChild">Does your child have allergies?
        <input type="radio" name="allergie" value="Yes" id="allergy-yes" bind:group={allergy}> Yes
        <input type="radio" name="allergie" value="No" id="allergy-no" bind:group={allergy}> No
      </label>
    
      {#if allergy === 'Yes'}
      <input class="wide-input" type="text" name="allergy-yes" id="allergy-id" required>
      {/if}
    
      <label for="medicationChild">Does your child take any medications?
        <input type="radio" name="medication" value="Yes" id="medication-yes" bind:group={medication}> Yes
        <input type="radio" name="medication" value="No" id="medication-no" bind:group={medication}> No
      </label>
    
      {#if medication === 'Yes'}
      <input class="wide-input" type="text" name="medication-yes" id="medication-id" required>
      {/if}

      <label for="activiesChild">What kind of activies does your child enjoy?
        <input placeholder="E.g. drawing, reading, going outside..." class="wide-input" type="text" name="activiesChild" id="activiesChild-id">
      </label>
    </div>
    

    <button type="submit"></button>
  </form>
</main>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}


