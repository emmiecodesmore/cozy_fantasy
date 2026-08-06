<script>
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import data from '../data/self_pub_by_year.json';

  const startYear = 2017;
  const shareData = data
    .filter(d => d.year >= startYear)
    .map(d => {
      const total = d["self-published"] + d["traditional"];
      return {
        year: d.year,
        pct_traditional: total > 0 ? (d["traditional"] / total) * 100 : 0,
        pct_self_pub: total > 0 ? (d["self-published"] / total) * 100 : 0,
      };
    });

  let crossoverYear = null;
  for (let i = 1; i < shareData.length; i++) {
    const prev = shareData[i - 1], curr = shareData[i];
    if (prev.pct_traditional < prev.pct_self_pub && curr.pct_traditional >= curr.pct_self_pub) {
      crossoverYear = curr.year;
    }
  }

  const width = 700, height = 400;
  const margin = { top: 30, right: 30, bottom: 40, left: 55 };

  const xScale = d3.scaleLinear().domain(d3.extent(shareData, d => d.year)).range([margin.left, width - margin.right]).nice();
  const yScale = d3.scaleLinear().domain([0, 100]).range([height - margin.bottom, margin.top]);
  const traditionalLine = d3.line().x(d => xScale(d.year)).y(d => yScale(d.pct_traditional));
  const selfPubLine = d3.line().x(d => xScale(d.year)).y(d => yScale(d.pct_self_pub));

  const xTicks = [...new Set(xScale.ticks(8).map(Math.round))];
  const yTicks = [0, 25, 50, 75, 100];

  let traditionalPathEl = null;
  let selfPubPathEl = null;
  let wrapperEl = null;
  let traditionalLength = 0;
  let selfPubLength = 0;
  let progress = 0;

  function updateProgress() {
    if (!wrapperEl) return;
    const rect = wrapperEl.getBoundingClientRect();
    const total = rect.height - window.innerHeight;
    if (total <= 0) { progress = 1; return; }
    progress = Math.min(1, Math.max(0, -rect.top / total));
  }

  onMount(() => {
    traditionalLength = traditionalPathEl.getTotalLength();
    selfPubLength = selfPubPathEl.getTotalLength();
    window.addEventListener('scroll', updateProgress);
    window.addEventListener('resize', updateProgress);
    updateProgress();
    return () => {
      window.removeEventListener('scroll', updateProgress);
      window.removeEventListener('resize', updateProgress);
    };
  });

  $: crossoverPoint = crossoverYear ? shareData.find(d => d.year === crossoverYear) : null;
</script>

<div class="scroll-wrapper" bind:this={wrapperEl}>
  <div class="sticky-chart">
    <section class="chart-section content-width">
      <h2>Self-Published Authors Led — Then Traditional Publishing Overtook Them</h2>
      <p class="chart-subtitle">Share of new cozy fantasy titles each year, by publishing path</p>

      <div class="legend">
        <span><i style="background: var(--color-rust)"></i> Traditional</span>
        <span><i style="background: var(--color-purple)"></i> Self-published</span>
      </div>

      <svg viewBox="0 0 {width} {height}">
        {#each yTicks as t}
          <line x1={margin.left} x2={width - margin.right} y1={yScale(t)} y2={yScale(t)} stroke="#eee" />
          <text x={margin.left - 10} y={yScale(t) + 4} font-size="10" text-anchor="end" fill="#888">{t}%</text>
        {/each}

        <path
          bind:this={traditionalPathEl}
          d={traditionalLine(shareData)}
          fill="none" stroke="var(--color-rust)" stroke-width="3"
          style="stroke-dasharray: {traditionalLength}; stroke-dashoffset: {traditionalLength * (1 - progress)};"
        />
        <path
          bind:this={selfPubPathEl}
          d={selfPubLine(shareData)}
          fill="none" stroke="var(--color-purple)" stroke-width="3"
          style="stroke-dasharray: {selfPubLength}; stroke-dashoffset: {selfPubLength * (1 - progress)};"
        />

        {#each xTicks as t}
          <text x={xScale(t)} y={height - 10} font-size="11" text-anchor="middle">{t}</text>
        {/each}

        {#if crossoverPoint && progress > 0.15}
            <circle cx={xScale(crossoverPoint.year)} cy={yScale(50)} r="5" fill="var(--color-brown, #3A2E28)" />
            <line
                x1={xScale(crossoverPoint.year)} y1={yScale(50)}
                x2={xScale(crossoverPoint.year) + 15} y2={yScale(50) - 40}
                stroke="var(--color-brown, #3A2E28)" stroke-width="1"
            />
            <foreignObject x={xScale(crossoverPoint.year) + 10} y={yScale(50) - 80} width="190" height="65">
                <div class="callout-panel">
                {crossoverPoint.year}: traditional overtakes self-pub
                </div>
            </foreignObject>
            {/if}
      </svg>
    </section>
    <p class="content-width">However, self-published Cozy Fantasy books are once again on the rise, so it will be interesting to see where the nascent genre will go in the following years, and how much more the ever-changing definition might adjust.</p>
  </div>
</div>

<style>
  .scroll-wrapper { height: 250vh; position: relative; }
  .sticky-chart { position: sticky; top: 10vh; }
  .chart-section { padding: 1rem 0; }
  .chart-subtitle { color: #888; margin-top: -0.5rem; }
  .legend { display: flex; gap: 1.5rem; margin: 0.5rem 0 1rem 0; font-size: 0.9rem; }
  .legend i { display: inline-block; width: 12px; height: 12px; border-radius: 50%; margin-right: 5px; }
  .callout-panel {
    background: var(--color-cream, #F5EDE1);
    border: 2px solid var(--color-brown, #3A2E28);
    border-radius: 8px;
    padding: 0.5rem 0.7rem;
    font-size: 0.78rem;
    line-height: 1.3;
    color: var(--color-brown, #3A2E28);
    box-sizing: border-box;
    }
  @keyframes fadeIn { to { opacity: 1; } }
</style>