<script>
  import { onMount } from "svelte";

  onMount(() => {
    const hamburgerIcon = document.getElementById("hamburger-icon");
    const hamburgerMenu = document.querySelector(".mobile");
    const closeIcon = document.getElementById("close-menu");
    const bodyEl = document.querySelector("body");

    if (hamburgerIcon && hamburgerMenu && closeIcon && bodyEl) {
      hamburgerIcon.addEventListener("click", () => {
        hamburgerMenu.classList.add("active");
        bodyEl.classList.add("freeze");
      });

      closeIcon.addEventListener("click", () => {
        hamburgerMenu.classList.remove("active");
        bodyEl.classList.remove("freeze");
      });

      document.querySelectorAll("ul.mobile li a").forEach((a) => {
        a.addEventListener("click", () => {
          hamburgerMenu.classList.remove("active");
          bodyEl.classList.remove("freeze");
        });
      });
    } else {
      console.error('Niet alle elementen zijn gevonden in de DOM.');
    }
  });
</script>


<header>
  <nav>
    <a href="/">
      <h1>
        <i class="fa-solid fa-person-breastfeeding"></i>In <span>Good</span> Hands
      </h1>
    </a>
    <ul class="desktop">
      <li>
        <a href="#about-us-link">about</a>
      </li>
      <li>
        <a href="#review-anchor">reviews</a>
      </li>
      <li>
        <a href="#FAQ-id">FAQ</a>
      </li>
      <li>
        <a href="#the-babysitters" class="CTA"
          ><i class="fa-solid fa-baby"></i> Book a babysitter</a
        >
      </li>
    </ul>

    <button type="button" id="hamburger-icon"
      ><i class="fa-solid fa-bars"></i></button
    >

    <ul class="mobile">
      <div class="top-menu">
        <h1><i class="fa-solid fa-person-breastfeeding"></i>In Good Hands</h1>
        <button type="button" id="close-menu"
          ><i class="fa-solid fa-xmark"></i></button
        >
      </div>
      <hr />
      <li>
        <a href="#about-us-link">about</a>
      </li>
      <hr />
      <li>
        <a href="#review-anchor">reviews</a>
      </li>
      <hr />
      <li>
        <a href="#FAQ-id">FAQ</a>
      </li>
      <hr />
      <li>
        <a href="#the-babysitters" class="CTA">Book a babysitter</a>
      </li>
      <hr />
    </ul>
  </nav>
</header>
