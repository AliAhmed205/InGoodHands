<script>
  import CtAs from "./CTAs.svelte";
import Cta from "./CTAs.svelte";
import Header from "./Header.svelte";
  import Reviews from "./Reviews.svelte";
import SectionWrapper from "./SectionWrapper.svelte";

</script>
<SectionWrapper>
  <Header/>
    <main class="homepage-section">
      <h2 class="slogan"><span>Trusted</span> Care for Your <span>Little Ones</span></h2>
      <div class="background-image"></div>
      <button id="live-chat"><i class="fa-regular fa-comment"></i></button>
      <CtAs/>
    </main>
</SectionWrapper>