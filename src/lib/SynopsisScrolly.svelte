<script>
  import { onMount } from 'svelte';
  import Decoration from './Decoration.svelte';
  import catImg from '../assets/decorations/cat.png';
  import mushroomImg from '../assets/decorations/mushroom.png';

  let activeStep = -1;

  onMount(() => {
    const triggers = document.querySelectorAll('.trigger');
    const observer = new IntersectionObserver(
      (entries) => {
        const visible = entries.filter(e => e.isIntersecting);
        if (visible.length > 0) {
          const mostVisible = visible.reduce((a, b) => a.intersectionRatio > b.intersectionRatio ? a : b);
          activeStep = Number(mostVisible.target.dataset.step);
        }
      },
      { threshold: [0.25, 0.5, 0.75] }
    );
    triggers.forEach((el) => observer.observe(el));
    return () => observer.disconnect();
  });
</script>

<section class="lead-in content-width" style="position: relative;">
  <Decoration src={catImg} top="105%" left="90%" size="355px" opacity={0.65} rotate="5deg" />
  <Decoration src={mushroomImg} top="25%" left="-15%" size="250px" opacity={0.85} rotate="-4deg" />
  <blockquote class="synopsis-quote">
    <p><span class="drop-cap">"A</span>fter a lifetime of bounties and bloodshed, Viv is hanging up her sword for the last time. The battle-weary orc aims to start fresh, opening the first ever coffee shop in the city of Thune. But old and new rivals stand in the way of success — not to mention the fact that no one has the faintest idea what coffee actually is. If Viv wants to put the blade behind her and make her plans a reality, she won't be able to go it alone. But the true rewards of the uncharted path are the travelers you meet along the way. And whether drawn together by ancient magic, flaky pastry, or a freshly brewed cup, they may become partners, family, and something deeper than she ever could have dreamed."</p>
  </blockquote>

  <p>Thus begins the synopsis of <em>Legends &amp; Lattes</em> by Travis Baldree, arguably the most famous book in the Cozy Fantasy genre.</p>

  <p>If you haven't heard of Cozy Fantasy, it wouldn't be a surprise. It's a relatively new genre, often considered a sub-genre of fantasy. And the definition of what actually counts as a cozy fantasy is still vehemently up for debate across the internet. But there are a few commonalities, and Legends and Lattes shares a few of those.</p>
</section>

<section class="scrolly">
  <div class="sticky-wrap content-width">
    <p class="synopsis pinned">
      "...whether drawn together by ancient
      <span class:highlight-gold={activeStep >= 0}>magic</span>, flaky pastry, or a freshly brewed cup, they may become
      <span class:highlight-purple={activeStep >= 1}>partners, family</span>, and
      <span class:highlight-green={activeStep >= 2}>the travelers you meet along the way</span>, and something
      <span class:highlight-pink={activeStep >= 3}>deeper</span> than she ever could have dreamed."
    </p>

    <div class="step-dots">
      {#each Array(5) as _, i}
        <span class="dot" class:active={activeStep === i}></span>
      {/each}
    </div>

    <div class="box-wrap">
      {#if activeStep === 0}
        <div class="box box-gold">
          <p>I set out to understand what really consitutes the cozy fantasy genre, and dive into some insights on common themes, how it came to be, and trends over the last few years. In order to do this, I hand-collected a list of 100 of the most-referenced cozy fantasy books across Goodreads, listicles, Amazon Books, and the r/cozyfantasy subreddit. Amongst that sample, <span class="highlight-gold">magic</span> is the term that appears most frequently in their descriptions, showing up 47% of the time.</p>
        </div>
      {:else if activeStep === 1}
        <div class="box box-purple">
          <p>Found <span class="highlight-purple">family</span> is one of the genre's most common threads. It often depicts characters building a chosen home together.</p>
        </div>
      {:else if activeStep === 2}
        <div class="box box-green">
          <p><span class="highlight-green">Friendship</span> shows up just as often. The travelers a protagonist meets along the way are frequently as central as their own arc.</p>
        </div>
      {:else if activeStep === 3}
        <div class="box box-pink">
          <p>And while <span class="highlight-pink">romance</span> is a common sub-plot, it's often not centered as the primary plot the way it is in many other genres.</p>
        </div>
      {:else if activeStep === 4}
        <div class="box box-rust closing-box">
          <p>When asked to describe the cozy fantasy genre, author Travis Baldree put it simply:</p>
          <p class="quote-text">"Stories that use fantasy to remind us why everyday things matter — and that leave us feeling better after we've read them."</p>
          <a class="source-link" href="https://worldbuildersclub.substack.com/p/what-is-cozy-fantasy-an-interview" target="_blank">Read the full interview →</a>
        </div>
      {/if}
    </div>
  </div>

  <div class="trigger" data-step="0"></div>
  <div class="trigger" data-step="1"></div>
  <div class="trigger" data-step="2"></div>
  <div class="trigger" data-step="3"></div>
  <div class="trigger" data-step="4"></div>
</section>

<section class="expand-out content-width">
  <p>But how do others describe this new genre, and where did it come from?</p>
</section>

<style>
  .lead-in p {
    margin-bottom: 1.25rem;
    line-height: 1.65;
    }
.synopsis-quote {
    margin-bottom: 1.5rem;
    line-height: 1.65;
    }
.lead-in {
    padding-top: 2rem;
    }
  .drop-cap {
    float: left;
    font-size: 3.5rem;
    line-height: 0.8;
    padding-right: 0.3rem;
    font-family: var(--font-heading);
    color: var(--color-rust);
  }
  .synopsis-quote {
    font-style: italic;
  }
  .scrolly { position: relative; }
  .sticky-wrap {
    position: sticky;
    top: 12vh;
  }
  .trigger { height: 100vh; }

  .step-dots { display: flex; gap: 0.5rem; margin: 1rem 0; }
  .dot { width: 8px; height: 8px; border-radius: 50%; background: #ddd; transition: background 0.3s; }
  .dot.active { background: var(--color-rust); }

  .box-wrap { min-height: 150px; }
  .box {
    padding: 1.25rem 1.5rem;
    border-radius: 12px;
    background: var(--color-cream);
    border: 2px solid;
    box-shadow: 0 4px 14px rgba(0,0,0,0.12);
  }
  .box-gold { border-color: var(--color-gold); }
  .box-purple { border-color: var(--color-purple); }
  .box-green { border-color: var(--color-green); }
  .box-pink { border-color: var(--color-pink); }
  .box-rust { border-color: var(--color-rust); }

  .closing-box { text-align: center; }
  .quote-text { font-style: italic; margin: 0.75rem 0; }
  .source-link {
    display: inline-block;
    margin-top: 0.5rem;
    padding: 0.4rem 1.1rem;
    border-radius: 999px;
    background: var(--color-rust);
    color: white;
    text-decoration: none;
    font-size: 0.9rem;
  }

  :global(.highlight-gold) { background: var(--color-gold); padding: 0 3px; border-radius: 3px; }
  :global(.highlight-purple) { background: var(--color-purple); padding: 0 3px; border-radius: 3px; }
  :global(.highlight-pink) { background: var(--color-pink); padding: 0 3px; border-radius: 3px; }
  :global(.highlight-green) { background: var(--color-green); padding: 0 3px; border-radius: 3px; color: white; }
</style>