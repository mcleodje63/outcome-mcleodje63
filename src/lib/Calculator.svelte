<!-- <script>
  let person = {
    firstName: "",
    lastName: "",
    age: 0,
  }

  function saveToLocal() {
    let data = JSON.stringify(person)
    localStorage.setItem("person", data)
  }

  function loadFromLocal() {
    let data = localStorage.getItem("person")
    person = JSON.parse(data)
  }
</script> -->
<!-- 
<form>
  <label>
    First name:
    <input bind:value={person.firstName} />
  </label>

  <label>
    Last name:
    <input bind:value={person.lastName} />
  </label>

  <label>
    Age:
    <input bind:value={person.age} />
  </label>
  <button on:click={saveToLocal}>Save</button>
  <button on:click={loadFromLocal}>Load</button>
</form>

<style>
  form {
    display: block;
    margin-bottom: 1em;
  }
</style> -->
