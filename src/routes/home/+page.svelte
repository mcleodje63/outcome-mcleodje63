<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<main>
  <Navigation />
  <Header />
  <span class="textBox"><p>Easier than ever before.</p></span>
  <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Add your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span>
  <span class="textRectangle"><p>No more stress over missed or late payments</p></span>
  <Footer />
</main>

<style>
  .textBox {
    height: 20vw;
    width: 20vw;
    background-color: #d0d6b3;
    border-radius: 25%;
    display: inline-block;
    margin: 2vw;
    /* what even is padding again? lol */
    padding: 25px;
  }
  .textRectangle {
    height: 7vw;
    width: 70vw;
    background-color: #b6adad;
    border-radius: 20%;
    /* fix this */
    display: inline-block;
    margin: 2vw;
    margin-left: 15%;
    padding: 25px;
  }
  p {
    color: white;
    font-family: "Inter", sans-serif;
    font-weight: bold;
    justify-content: center;
    font-size: 2em;
  }
</style>
