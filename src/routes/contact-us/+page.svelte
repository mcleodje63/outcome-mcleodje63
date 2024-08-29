<script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
</script>

<Navigation />
<main>
  <div class="textContainer">
    <p>Email us at <b>flatteremail@gmail.com</b></p>
    <p>Or</p>
    <p>Reach out on <a href="https://www.linkedin.com/in/flatter-admin-201904324/">LinkedIn</a></p>
  </div>
</main>
<Footer />

<style>
  p {
    font-weight: 400;
    margin: 5vw;
    color: #6e6e6e;
    font-size: 2.5vw;
    margin-bottom: 10vw;
    margin-top: 10vw;
  }
  .textContainer {
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
</style>
