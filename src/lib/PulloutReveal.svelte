<script>
  import { onMount } from 'svelte';

  export let plainImg;
  export let annotatedImg;
  export let title;

  let sectionEl = null;
  let revealed = false;

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting) {
          revealed = true;
          observer.disconnect();
        }
      },
      { threshold: 0.2, rootMargin: '0px 0px -10% 0px' }
    );
    observer.observe(sectionEl);
    setTimeout(() => { revealed = true; }, 1500);
    return () => observer.disconnect();
  });
</script>

<div class="pullout-reveal" bind:this={sectionEl}>
  <div class="image-stack">
    <img src={plainImg} alt={title} class="base-img" />
    <img src={annotatedImg} alt={`Annotated: ${title}`} class="annotated-img" class:visible={revealed} />
  </div>
  <p class="pullout-caption"><em>{title}</em></p>
</div>

<style>
  .pullout-reveal {
    max-width: 480px;
    margin: 0 auto 3rem auto;
    padding: 0.5rem 0.5rem 0.75rem 0.5rem;
    border-radius: 20px 220px 20px 220px / 220px 20px 220px 20px;
    border: 2px solid var(--color-gold);
    background: var(--color-cream);
    box-shadow: 7px 7px 0 var(--color-gold);
    text-align: center;
  }
  .image-stack { position: relative; line-height: 0; }
  .base-img, .annotated-img {
    width: 100%;
    display: block;
    border-radius: 14px 200px 14px 200px / 200px 14px 200px 14px;
  }
  .annotated-img {
    position: absolute;
    top: 0;
    left: 0;
    clip-path: inset(0 100% 0 0);
    transition: clip-path 2s ease;
  }
  .annotated-img.visible {
    clip-path: inset(0 0 0 0);
  }
  .pullout-caption { margin: 0.5rem 0 0 0; font-style: italic; }
</style>