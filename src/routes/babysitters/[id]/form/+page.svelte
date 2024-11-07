<script>
  import { page } from "$app/stores";
  import { onMount } from "svelte";
  import Header from "../../../../components/Header.svelte";
  import "../form/client-form.css";

  let babysitter = null;
  let loading = true;

  let allergy = null;
  let medication = null;
  let ages = Array.from({ length: 18 }, (_, i) => i);

  let selectedDay = "";
  let selectedTime = "";

  const { id } = $page.params;

  const days = {
    mo: "Monday",
    tue: "Tuesday",
    wen: "Wednesday",
    thu: "Thursday",
    fri: "Friday",
    sat: "Saturday",
    sun: "Sunday"
  }

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
      console.error(
        "Er was een probleem met het ophalen van de babysitter data:",
        error
      );
    } finally {
      loading = false;
    }
  });

  function handleSubmit() {
    const successContainer = document.querySelector(".succes-container");
    successContainer.classList.add("active");

    setTimeout(() => {
      window.location.href = "/";
    }, 4000); 
  }
</script>

{#if loading}
  <p>Loading...</p>
{:else if babysitter}
  <Header />
  <main class="book-form">
    <section class="form-container">
      <form action="/">
        <h1>Book {babysitter.naam}</h1>
        <p>Excellent choice! {babysitter.naam} will be your extra pair of hands, looking after your children. Please fill out the form with as much detail as possible. Once you've sent your request, {babysitter.naam} will seek contact with you.</p>
        <h2><i class="fa-solid fa-user"></i> Parent's Contact</h2>
        <article>
          <div>
            <label for="userName"
              >First Name
              <input type="text" name="userName" id="userName" required />
            </label>

            <label for="surName"
              >Last Name
              <input type="text" name="surName" id="surName" required />
            </label>

            <label for="phoneNumber"
              >Phone Number
              <input type="number" name="phoneNumber" id="phoneNumber" />
            </label>

            <label for="userEmail"
              >E-mail
              <input id="userEmail" type="email" name="userEmail" required />
            </label>
          </div>
          <div>
            <label for="userEmail"
              >City
              <select name="userCity" id="userCity-id" required>
                <option>Kies een stad</option>
                <option value="Amsterdam">Amsterdam</option>
                <option value="Rotterdam">Rotterdam</option>
                <option value="Maastricht">Maastricht</option>
                <option value="Eindhoven">Eindhoven</option>
                <option value="Den Haag">Den Haag</option>
              </select>
            </label>
            <label for="userEmail"
              >Adress
              <input id="userEmail" type="email" name="userEmail" required />
            </label>
            <label for="userEmail"
              >Zip-code
              <input id="userEmail" type="email" name="userEmail" required />
            </label>
          </div>
        </article>

        <h2><i class="fa-solid fa-child"></i> Child's Information</h2>
        <article>
          <div>
          <label for="userName"
            >First Name
            <input type="text" name="userName" id="userName" required />
          </label>

          <label for="surName"
            >Last Name
            <input type="text" name="surName" id="surName" required />
          </label>
          <label for="selectAge"
            >Age
            <select name="selectAge" id="selectAge" required>
              {#each ages as age}
                <option value={age}>{age}</option>
              {/each}
            </select>
          </label>
        </div>
        <div>
          <label for="allergyChild"
            >Does your child have allergies?
            <div class="multiple-choice-container">
              <input
                type="radio"
                name="allergie"
                value="Yes"
                id="allergy-yes"
                bind:group={allergy}
              />
              Yes
              <input
                type="radio"
                name="allergie"
                value="No"
                id="allergy-no"
                bind:group={allergy}
              /> No
            </div>
          </label>

          {#if allergy === "Yes"}
            <input
              class="wide-input"
              type="text"
              name="allergy-yes"
              id="allergy-id"
              required
            />
          {/if}

          <div class="multiple-choice-container">
            <label for="medicationChild"
              >Does your child take any medications?
              <div class="multiple-choice-container">
                <input
                  type="radio"
                  name="medication"
                  value="Yes"
                  id="medication-yes"
                  bind:group={medication}
                />
                Yes
                <input
                  type="radio"
                  name="medication"
                  value="No"
                  id="medication-no"
                  bind:group={medication}
                /> No
              </div>
            </label>
          </div>

          {#if medication === "Yes"}
            <input
              class="wide-input"
              type="text"
              name="medication-yes"
              id="medication-id"
              required
            />
          {/if}

          <label for="activiesChild"
            >What kind of activies does your child enjoy?
            <input
              placeholder="E.g. drawing, reading, going outside..."
              class="wide-input"
              type="text"
              name="activiesChild"
              id="activiesChild-id"
            />
          </label>
        </div>
        </article>
        <h2><i class="fa-regular fa-calendar"></i> Date</h2>
        <article>
          <div>
            <label for="day">Choose day</label>
          <select id="day" name="day" bind:value={selectedDay} required>
            <option value="" disabled selected>-</option>
            {#each Object.keys(babysitter.beschikbaarheid) as dag}
              {#if babysitter.beschikbaarheid[dag].morning || babysitter.beschikbaarheid[dag].noon || babysitter.beschikbaarheid[dag].afternoon || babysitter.beschikbaarheid[dag].evening}
              <option value={dag}>{days[dag]}</option>
              {/if}
            {/each}
          </select>

          {#if selectedDay}
            <label for="time" id="choose-time">Choose a timeslot</label>
            <select id="time" name="time" required>
              <option value="" disabled selected>-</option>
              {#if babysitter.beschikbaarheid[selectedDay].morning}
                <option value="ochtend">Morning</option>
              {/if}
              {#if babysitter.beschikbaarheid[selectedDay].noon}
                <option value="middag">Noon</option>
              {/if}
              {#if babysitter.beschikbaarheid[selectedDay].afternoon}
                <option value="namiddag">Afternoon</option>
              {/if}
              {#if babysitter.beschikbaarheid[selectedDay].evening}
                <option value="avond">Evening</option>
              {/if}
            </select>
          {/if}
        </div>
        </article>
        <div class="choice-container">
        <button type="button" on:click={handleSubmit}><i class="fa-solid fa-envelope"></i> Submit to {babysitter.naam}</button>
        <a href="/"><i class="fa-solid fa-xmark"></i> Cancel</a>
      </div>
      </form>
    </section>
    <div class="succes-container">
      <i class="fa-solid fa-check"></i>
      <h3>Thank you!</h3>
      <h4>Your request is in good hands!</h4>
    </div>
  </main>
{:else}
  <p>Babysitter niet gevonden.</p>
{/if}
