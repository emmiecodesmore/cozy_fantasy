<section class="methodology content-width">
  <h2>Methodology & Sources</h2>
  <p>I built this project's dataset from scratch. Since the Goodreads API is no longer publicly available, I hand-compiled a list of roughly 100 cozy fantasy titles by cross-referencing Goodreads top Cozy Fantasy Lists and tags, Amazon's Cozy Fantasy bestseller lists, and the subreddit r/cozyfantasy. I deliberately pulled from a mix of sources so the dataset wasn't skewed toward only bestsellers or only traditional publishing.</p>
  <p>From there, I used the Google Books and Open Library APIs to pull metadata: publisher, publication date, and cover art, for each title, working through the data in Python and Pandas inside Jupyter notebooks. Whether a book was self-published or traditionally published was determined using a publisher-name heuristic (matching against known imprints), with manual spot-checks for anything ambiguous.</p>
  <p>The site itself was built in Svelte, with charts in D3 and illustrations/annotations done in Adobe Illustrator and Canva.</p>
  <p class="credit">By Emmie King </p>
</section>

<style>
  .methodology { padding: 3rem 1.5rem; border-top: 2px solid var(--color-gold); font-size: 0.9rem; color: #555; }
  .methodology h2 { font-size: 1.3rem; }
  .credit { margin-top: 1.5rem; font-style: italic; }
</style>