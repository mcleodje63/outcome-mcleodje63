<!-- <script>
  import Footer from "$lib/Footer.svelte"
  import Navigation from "$lib/Navigation.svelte"
  document.getElementById("emailForm").addEventListener("submit", async function (e) {
    e.preventDefault()

    const name = document.getElementById("name").value
    const email = document.getElementById("email").value
    const message = document.getElementById("message").value

    const response = await fetch("/send-email", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ name, email, message }),
    })

    const result = await response.json()
    document.getElementById("response").textContent = result.message
  })
</script>

<Navigation />

<main>
  <div class="textContainer">
    <p>HELLO TESTING</p>
    <div class="form-container">
      <h2>Contact us for help</h2>
      <form id="emailForm">
        <input type="text" id="name" placeholder="Your name" required />
        <input type="email" id="email" name="email" placeholder="Your Email" required />
        <textarea id="message" name="message" placeholder="Your Message" required></textarea>
        <button type="submit">Send</button>
      </form>
      <p id="response"></p>
    </div>
    <script src="script.js"></script>
  </div>
</main>
<Footer />

<style>
  p {
    text-align: center;
    margin: 20vw;
  }
  body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f4f4f9;
  }

  .form-container {
    width: 300px;
    padding: 20px;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  h2 {
    margin-bottom: 20px;
    color: #333;
  }

  input,
  textarea {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  button {
    width: 100%;
    padding: 10px;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background: #0056b3;
  }

  #response {
    color: red;
    margin-top: 10px;
  }
</style> -->
