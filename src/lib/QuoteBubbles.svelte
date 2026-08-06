<script>
 
  const quotes = [
    { text: "The genre of imaginative fiction involving magic and adventure that gives a feeling of comfort, warmth, and relaxation", source: "r/cozyfantasy official subreddit definition" },
    { text: "To me, the defining element is that the magic/world serves the character’s inner life rather than threatening it. The tension is emotional and relational, not existential.", source: "u/mjgillbright, r/cozyfantasy" },
    { text: "As you get to the edges of the genre and stop picking up books about retired adventures opening up drink shops, you’re going to find ones that are going to be cozy to some and not to others. And that’s okay. We don’t need to split every single hair down to the molecule", source: "u/action_lawyer_comics, r/cozyfantasy" },
    { text: "Not to over analyze, but I feel like the “best” cozy stories usually have cozy elements set against a distinctly non-cozy elements to achieve contrast. Which non-cozy elements are acceptable is completely up to the individual.", source: "u/justascarecrow1, r/cozyfantasy" },
  ];
</script>

<section class="quotes content-width">
  <h2>So, What Actually IS Cozy Fantasy?</h2>
  {#each quotes as q, i}
    <div class="quote-card" style="--rot: {i % 2 === 0 ? '-2deg' : '2deg'}">
      <p>"{q.text}"</p>
      <span class="source">— {q.source}</span>
    </div>
  {/each}
  <p class="transition-line">That's how people tend to describe the genre, but what can we learn from the books themselves?</p>
</section>

<style>
  .quotes { padding: 2rem 1.5rem; }
  .quote-card {
    background: var(--color-cream);
    border: 2px solid var(--color-gold);
    border-radius: 255px 15px 225px 15px / 15px 225px 15px 255px;
    box-shadow: 6px 6px 0 var(--color-gold);
    padding: 1.25rem 1.5rem;
    max-width: 420px;
    margin: 0 auto 2rem auto;
    transform: rotate(var(--rot));
  }
  .quote-card:nth-child(3n) { border-color: var(--color-purple); box-shadow: 6px 6px 0 var(--color-purple); }
  .quote-card:nth-child(3n+1) { border-color: var(--color-rust); box-shadow: 6px 6px 0 var(--color-rust); }
  .source { display: block; margin-top: 0.5rem; font-size: 0.85rem; color: #888; font-style: italic; }
  .transition-line { text-align: center; margin-top: 2.5rem; font-weight: 600; }
</style>