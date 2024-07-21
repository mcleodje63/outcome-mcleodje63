<script>
  import Header from "$lib/Header.svelte"
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<main>
  <Navigation />
  <span class="container">
    <img src="citySkyline.JPG" style="max-width: 100%;" alt="Singapore buildings" />
  </span>
  <div class="header">
    <Header title="Welcome to Flatter" subtitle="Rent made easy" />
  </div>

  <span class="textBox"><p>Easier than ever before.</p></span>
  <span class="textBox"><p>Simple and accurate.</p></span>
  <span class="textBox"><p>Work out your flat now.</p></span>
  <span class="textBox"><p>Made for students, by students.</p></span>
  <span class="textRectangle"><p>No more stress over missed or late payments</p></span>
  <Footer />
</main>

<style>
  .container {
    position: relative;
    text-align: center;
    color: white;
    padding: 0;
  }

  .header {
    position: absolute;
    top: 12.5vw;
    left: 1vw;
  }
  .textBox {
    height: 20vw;
    width: 20vw;
    background-color: #d0d6b3;
    border-radius: 25%;
    display: inline-flex;
    margin: 2vw;
    padding: 25px;
  }
  .textBox:hover {
    background: #d0d6b3ad;
  }
  .textRectangle {
    height: 7vw;
    width: 70vw;
    background-color: #b6adad;
    border-radius: 25px;
    display: inline-flex;
    margin: 2vw;
    margin-left: 15%;
    padding: 1.7vw;
    text-align: center;
    justify-content: center;
  }

  .textRectangle:hover {
    background: #c6bdbd;
  }
  p {
    color: white;
    font-family: "Inter", sans-serif;
    justify-content: center;
    font-size: 2.5vw;
    font-weight: 600;
  }
</style>
