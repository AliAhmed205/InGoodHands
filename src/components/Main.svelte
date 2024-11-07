<script>
  import HomepageSection from "./Homepage-section.svelte";
  import About from "./About.svelte";
  import Reviews from "./Reviews.svelte";
  import FAQ from "./FAQ.svelte";
  import Form from "./Form.svelte";
  import Babysitters from "./Babysitters.svelte";

  import "../components/CSS/Homepage.css"
  import "../components/CSS/About.css"
  import "../components/CSS/Babysitters.css"
  
</script>

<main>
  <HomepageSection /> 
  <About />
  <Babysitters/>
  <Reviews />
  <FAQ /> 
  <Form />
</main>