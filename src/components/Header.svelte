<script>
  import { onMount } from "svelte";
  import Dock from "./Dock.svelte";
  import HamburgerMenu from "./HamburgerMenu.svelte";

  onMount(() =>{
    const hamburgerIcon = document.getElementById('hamburger-icon')
    const menu = document.getElementById('menu-section')
    const closeBtn = document.getElementById('close-icon')
    const logoMenu = document.getElementById('logo-menu')

    if(hamburgerIcon) {
      hamburgerIcon.addEventListener("click", ()=>{
        menu.classList.add('active')
      })

      closeBtn.addEventListener('click', ()=>{
        menu.classList.remove('active')
      })

      logoMenu.addEventListener('click', ()=>{
        menu.classList.remove('active')
      })
    }
  })

</script>

<header class="main-header">
    <a href="/">
      <h1><i class="fa-solid fa-person-breastfeeding"></i>
        In <span>Good</span> 
        Hands
      </h1>
    </a>
    <nav>
      <ul>
      <li><a href="#About">about</a></li>
      <li><a href="#ourTeam">our team</a></li>
      <li><a href="#FAQs">FAQs</a></li>
      <li><a href="#reviews">reviews</a></li>
      <button class="specialBtn"><i class="fa-solid fa-baby"></i>Book a babysitter</button>
    </ul>
    </nav>
    <button type="button" id="hamburger-icon"><i class="fa-solid fa-bars"></i></button>
    <Dock />
    <HamburgerMenu />
</header>
