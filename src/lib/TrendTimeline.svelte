<script>
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import timelineCounts from '../data/timeline_counts.json';
  //import crossGenreTrends from '../data/cross_genre_trends.json';

  const crossGenreModules = import.meta.glob('../data/cross_genre_trends.json', { eager: true });
  const crossGenreTrends = crossGenreModules['../data/cross_genre_trends.json']?.default ?? [];
  const startYear = 2015;
  const filteredData = timelineCounts.filter(d => d.year >= startYear);

  const baseline = filteredData[0].count;
  const cozyIndexed = filteredData.map(d => ({ year: d.year, value: (d.count / baseline) * 100 }));

  const firstVal = cozyIndexed[0]?.value ?? 100;
  const lastVal = cozyIndexed[cozyIndexed.length - 1]?.value ?? 100;
  const growthPct = Math.round(lastVal - firstVal);

  const genres = ['cozy mystery', 'cozy sci-fi', 'cozy horror'];
  const genreColors = ['var(--color-purple)', 'var(--color-pink)', 'var(--color-brown)'];
  const otherGenres = genres.map(genre => {
    const series = crossGenreTrends.filter(d => d.genre === genre && d.year >= startYear);
    const base = series[0]?.interest ?? 1;
    return { genre, values: series.map(d => ({ year: d.year, value: (d.interest / base) * 100 })) };
  });

  const hasCrossGenreData = otherGenres.some(g => g.values.length > 0);

  const width = 700, height = 400;
  const margin = { top: 40, right: 30, bottom: 40, left: 60 };

  const xScale = d3.scaleLinear().domain(d3.extent(filteredData, d => d.year)).range([margin.left, width - margin.right]).nice();
  const yScale = d3.scaleLinear()
    .domain([0, d3.max([...cozyIndexed, ...otherGenres.flatMap(g => g.values)], d => d.value)])
    .range([height - margin.bottom, margin.top]).nice();
  const lineGen = d3.line().x(d => xScale(d.year)).y(d => yScale(d.value));

  const cozyPath = lineGen(cozyIndexed);
  const xTicks = [...new Set(xScale.ticks(8).map(Math.round))];
  const yTicks = yScale.ticks(5);

  const annotations = [
    { year: 2020, label: "The Covid-19 pandemic drives a desire for cozy reading" },
    { year: 2022, label: "Legends & Lattes goes viral" },
  ];

  let pathEl = null;
  let wrapperEl = null;
  let pathLength = 0;
  let progress = 0;

  $: lineProgress = Math.min(1, progress / 0.45);          // 0–45% of scroll: draw the cozy fantasy line
  $: showAnnotations = progress > 0.4;                        // 40%+: annotations appear
  $: genreProgress = Math.max(0, Math.min(1, (progress - 0.55) / 0.35)); // 55–90%: other genres fade in

  function updateProgress() {
    if (!wrapperEl) return;
    const rect = wrapperEl.getBoundingClientRect();
    const total = rect.height - window.innerHeight;
    if (total <= 0) { progress = 1; return; }
    progress = Math.min(1, Math.max(0, -rect.top / total));
  }

  onMount(() => {
    pathLength = pathEl.getTotalLength();
    window.addEventListener('scroll', updateProgress);
    window.addEventListener('resize', updateProgress);
    updateProgress();
    return () => {
      window.removeEventListener('scroll', updateProgress);
      window.removeEventListener('resize', updateProgress);
    };
  });

  function findValue(year) {
    const match = cozyIndexed.find(d => Number(d.year) === Number(year));
    return match ? match.value : 0;
  }
</script>

<div class="scroll-wrapper" bind:this={wrapperEl}>
  <div class="sticky-chart">
    <section class="chart-section content-width">
        <h2>Cozy Fantasy Publishing Has Grown <span class="stat-highlight">{growthPct}%</span> Since {startYear}</h2>
        <p>This genre has grown significantly more popular in recent years. This shows the overall trend for the number of books within my dataset over the last eleven years.</p>
        <svg viewBox="0 0 {width} {height}">
        {#each yTicks as t}
          <line x1={margin.left} x2={width - margin.right} y1={yScale(t)} y2={yScale(t)} stroke="#eee" />
          <text x={margin.left - 10} y={yScale(t) + 4} font-size="10" text-anchor="end" fill="#888">{t}</text>
        {/each}
        
        <rect
          x={xScale(2020)} width={xScale(2022) - xScale(2020)}
          y={margin.top} height={height - margin.top - margin.bottom}
          fill="var(--color-gold)" opacity="0.15"
        />
        <text x={(xScale(2020) + xScale(2022)) / 2} y={margin.top - 10} font-size="11" text-anchor="middle" fill="var(--color-brown)">Pandemic era</text>

        <path
          bind:this={pathEl}
          d={cozyPath}
          fill="none" stroke="var(--color-green)" stroke-width="3"
          style="stroke-dasharray: {pathLength}; stroke-dashoffset: {pathLength * (1 - lineProgress)};"
        />

        {#each xTicks as t}
          <text x={xScale(t)} y={height - 10} font-size="11" text-anchor="middle">{t}</text>
        {/each}
        </svg>

        {#if genreProgress > 0.3 && hasCrossGenreData}
            <div class="legend" style="opacity: {genreProgress}">
            <span><i style="background: var(--color-green)"></i> Cozy fantasy</span>
            <!-- {#each genres as genre, i}
                <span><i style="background: {genreColors[i]}"></i> {genre}</span>
            {/each} -->
            </div>
        {/if}

      {#if genreProgress > 0.3}
        <div class="legend" style="opacity: {genreProgress}">
          <span><i style="background: var(--color-green)"></i> Cozy fantasy</span>
          {#each genres as genre, i}
            <span><i style="background: {genreColors[i]}"></i> {genre}</span>
          {/each}
        </div>
      {/if}
      <p>You can see the sharp increase in cozy fantasy titles in recent years, along with a couple of pivotal moments highlighted. </p>
      <p>Next, we'll look at an important discussion within the world of Cozy Fantasy: the self-published vs. traditional publishing path. <em>Legends & Lattes</em> was originally self-published by Travis Baldree, and only later was picked up by a traditional publisher, and that trend isn't unique. You can see in the following chart that this was a situation where self-published cozy fantasy novels led the surge, but traditionally published books in the genre were quick to follow.</p>  
    </section>
  </div>
</div>

<style>
  .scroll-wrapper { height: 280vh; position: relative; }
  .sticky-chart { position: sticky; top: 10vh; }
  .chart-section { padding: 1rem 0; }
  .chart-subtitle { color: #888; margin-top: -0.5rem; }
  .legend { display: flex; gap: 1rem; margin-top: 1rem; font-size: 0.85rem; flex-wrap: wrap; transition: opacity 0.3s; }
  .legend i { display: inline-block; width: 10px; height: 10px; border-radius: 50%; margin-right: 4px; }
  @keyframes fadeIn { to { opacity: 1; } }
</style>