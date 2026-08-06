<script>
import PulloutReveal from './PulloutReveal.svelte';
import Decoration from './Decoration.svelte';
import plain1 from '../assets/covers/legends_and_lattes_plain.png';
import annotated1 from '../assets/covers/legends_and_lattes_annotation.png';
import plain2 from '../assets/covers/teller_of_small_fortunes_plain.png';
import annotated2 from '../assets/covers/teller_of_small_fortunes_annotation.png';
import plain3 from '../assets/covers/rewitched_plain.png';
import annotated3 from '../assets/covers/rewitched_annotation.png';
import plain4 from '../assets/covers/violet_thistlewaite_plain.png';
import annotated4 from '../assets/covers/violet_thistlewaite_annotation.png';
import collageImg from '../assets/covers/Cover_collage.png';
import cottageImg from '../assets/decorations/cottage.png';

const pullouts = [
    { plain: plain1, annotated: annotated1, title: 'Legends & Lattes' },
    { plain: plain2, annotated: annotated2, title: 'The Teller of Small Fortunes' },
    { plain: plain3, annotated: annotated3, title: 'Rewitched' },
    { plain: plain4, annotated: annotated4, title: 'Violet Thistlewaite' }
];
</script>

<section class="covers content-width" style="position: relative;">
    <h2>A Selection of Cozy Fantasy Book Covers</h2>
    <p class="intro">Below is a selection of covers drawn from the 100-book Cozy Fantasy dataset I built for this project, pulled from the most-referenced titles across Goodreads, listicles, Amazon Books, and r/cozyfantasy. See if you can spot any common motifs or visual themes across these covers, then scroll down to see a few examples.</p>
    <Decoration src={cottageImg} top="10%" left="95%" size="200px" opacity={0.85} rotate="-1deg" />
    <img src={collageImg} alt="Grid of cozy fantasy book covers" class="collage" />
</section>

<section class="content-width">
    <h2 class="pullout-header">Let's dive into a few of these covers with more detail, to look at the common motifs visually represented there</h2>
    {#each pullouts as p}
        <PulloutReveal plainImg={p.plain} annotatedImg={p.annotated} title={p.title} />
    {/each}
</section>

<style>
.collage {
    width: 100%;
    border-radius: 12px;
    margin-bottom: 2rem;
}
.intro {
    max-width: 700px;
}
.pullout-header {
    text-align: center;
    max-width: 700px;
    margin: 0 auto 2rem auto;
}
</style>