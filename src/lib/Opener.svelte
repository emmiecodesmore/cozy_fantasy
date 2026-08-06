<script>
  import heroImg from '../assets/hero.png';
  import Decoration from './Decoration.svelte';
  import teapotImg from '../assets/decorations/teapot.png';
  import dragonImg from '../assets/decorations/dragon.png';
</script>

<section class="hero">
    <!-- <Decoration src={teapotImg} top="1%" left="42%" size="650px" opacity={0.70} rotate="-6deg" /> -->
    <Decoration src={dragonImg} top="80%" left="8%" size="550px" opacity={0.75} rotate="10deg" />
    <img src={heroImg} alt="The Rise of Cozy Fantasy, by Emmie King" class="hero-cover" />
</section>

<style>
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--color-green);
    padding: 3rem 1rem;
    position: relative;
  }
  .hero-cover {
    max-height: 90vh;
    max-width: 90vw;
    width: auto;
    height: auto;
    object-fit: contain;
  }
</style>
