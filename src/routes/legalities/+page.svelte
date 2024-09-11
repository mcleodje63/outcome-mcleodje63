<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
  import Header from "$lib/Header.svelte"
</script>

<Navigation />

<main>
  <div class="headerContainer">
    <span class="imgContainer">
      <img src="yellowHouse.JPG" alt="Yellow character house" />
      <!-- is this meant to have gaps?  -->
    </span>
    <div class="header">
      <Header title="The legal stuff" subtitle="We'll worry about that." />
    </div>
  </div>
  <div class="textContainer">
    <p>Template <a href="https://github.com/craighead-diocesan-school/sveltekit-template.git"><b>sveltekit-template</b> </a>by <a href="https://github.com/MrHullen"><b>Mr Hullen</b></a></p>

    <p>Icons made by <a href="https://www.flaticon.com/authors/freepik" title="Freepik"> Freepik </a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></p>

    <p>
      Pinterest icons created by <a href="https://www.flaticon.com/free-icons/pinterest" title="pinterest icons">Pixel perfect - Flaticon</a>
    </p>
    <p>
      All photos by <a href="https://nz.pinterest.com/jessieeemcleod/?invite_code=e72319c28e8c4f84baba0bae3aca7bac&sender=849913898336554695" title="pinterest icons">Jessie McLeod</a>
    </p>
  </div>
</main>
<Footer />

<style>
  p {
    text-align: center;
    font-size: 2vh;
    margin-bottom: 2vw;
  }
  .textContainer {
    margin: 20vw;
  }
  .headerContainer {
    font-family: "Inter", sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 45vw;
    margin: 0;
    background-color: #f4f4f9;
    position: relative;
  }
  .imgContainer {
    width: 100%;
    overflow: hidden;
    position: reletive;
  }
  .imgContainer img {
    width: 100%;
    height: auto; /* Maintain aspect ratio */
    display: block;
  }
  .header {
    position: absolute;
    top: 15vw;
    left: 30vw; /* Center the header */
    text-align: center;
    color: white;
  }
</style>
