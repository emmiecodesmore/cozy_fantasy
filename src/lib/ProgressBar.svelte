<script>
  import { onMount } from 'svelte';
  import teapotImg from '../assets/decorations/teapot.png';

  let progress = 0;
  let showBackToTop = false;

  function updateProgress() {
    const scrollTop = window.scrollY;
    const docHeight = document.documentElement.scrollHeight - window.innerHeight;
    progress = docHeight > 0 ? scrollTop / docHeight : 0;
    showBackToTop = scrollTop > 600;
  }

  function scrollToTop() {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  onMount(() => {
    window.addEventListener('scroll', updateProgress);
    updateProgress();
    return () => window.removeEventListener('scroll', updateProgress);
  });
</script>

<div class="progress-track">
  <div class="progress-fill" style="width: {progress * 100}%"></div>
  <img src={teapotImg} alt="" class="progress-icon" style="left: calc({progress * 100}% - 15px)" />
</div>

{#if showBackToTop}
  <button class="back-to-top" on:click={scrollToTop} aria-label="Back to top">↑</button>
{/if}

<style>
  .progress-track { position: fixed; top: 0; left: 0; right: 0; height: 6px; background: rgba(0,0,0,0.05); z-index: 1000; }
  .progress-fill { height: 100%; background: var(--color-gold); transition: width 0.1s linear; }
  .progress-icon { position: absolute; top: -12px; width: 30px; height: 30px; transition: left 0.1s linear; }
  .back-to-top {
    position: fixed; bottom: 2rem; right: 2rem; width: 48px; height: 48px; border-radius: 50%;
    background: var(--color-rust); color: white; border: none; font-size: 1.3rem;
    cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.2); z-index: 1000;
  }
</style>