<script>
  import Header from '$lib/Header.svelte'
  import Footer from '$lib/Footer.svelte'
</script>

<Header />

<main><div class="columns">
  <h2>SvelteKit</h2>

  <p>Welcome to coding with SvelteKit, a modern JavaScript framework that makes it easy to code great apps.</p> 
  </div>
</main>

<Footer />

<style>
main{padding-bottom:3rem;
}
.columns{display:flex;
align-items:center;
justify-content: space-evenly;
padding: 5rem 10rem;}
</style>
