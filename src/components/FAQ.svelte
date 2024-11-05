<script>
  import { onMount } from "svelte";

  onMount(() => {
    document.querySelectorAll(".accordion-button").forEach((button) => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".accordion-content").forEach((content) => {
          if (content !== button.nextElementSibling) {
            content.style.maxHeight = null;
          }
        });

        const content = button.nextElementSibling;
        if (content.style.maxHeight === "160px") {
          content.style.maxHeight = null;
        } else {
          content.style.maxHeight = "160px";
        }
      });
    });
  });
</script>

<section class="FAQ-section" id="FAQ-id">
  <h2>FAQ</h2>
  <div class="accordion">
    <button class="accordion-button"
      ><i class="fa-regular fa-circle-question"></i> Is there an age limit fot us
      to babysit ?</button
    >
    <div class="accordion-content">
      <p>
        Yes, we do have an age limit for babysitting, which is up to 13 years
        old. However, we are flexible and can accommodate children with special
        needs beyond that age.
      </p>
    </div>
    <button class="accordion-button"
      ><i class="fa-regular fa-circle-question"></i> What activities do you do with
      the kids?</button
    >
    <div class="accordion-content">
      <p>
        We engage in a variety of activities like games, crafts, and outdoor
        play.
      </p>
    </div>
    <button class="accordion-button"
      ><i class="fa-regular fa-circle-question"></i> Can you follow specific routines?</button
    >
    <div class="accordion-content">
      <p>Yes, we can follow any specific routines or requests you have.</p>
    </div>
  </div>
</section>
