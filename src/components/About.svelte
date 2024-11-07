<script>
  import { onMount } from "svelte";

  let team = [];

  onMount(async () => {
    try {
      const response = await fetch(
        "https://raw.githubusercontent.com/AliAhmed205/sourceOefening/main/InGoodHands/babysitter.json"
      );
      const data = await response.json();

      team = data.babysitter_service.team;
    } catch (error) {
      console.error("Fout bij het ophalen van de teamgegevens:", error);
    }
  });
</script>

<section class="about-section" id="about-us-link">
  <h2>About us</h2>
  <p>
    <b>In Good Hands</b> provides trusted, reliable, and caring babysitting services,
    ensuring your little ones are safe, happy, and well-cared for. Our experienced
    sitters prioritize your child's comfort and well-being, offering peace of mind
    for parents.
  </p>
  <ul class="about-us">
    {#each team as member}
      <li>
        <img src={member.avatar} alt={member.naam} />
        <h3>{member.naam}</h3>
        <p>{member.functie}</p>
      </li>
    {/each}
  </ul>
</section>
