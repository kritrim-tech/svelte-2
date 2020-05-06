<script>
  let files;
  let bening;
  let malignant;

  const url = "https://skin-cancer-api.herokuapp.com/predict";

  const draw = () => {
    var img = document.getElementById("image");
    document.getElementById("button").disabled = false;
    img.src = URL.createObjectURL(files[0]);
    bening = null;
    malignant = null;
  };

  const predict = () => {
    document.getElementById("button").disabled = true;

    let formData = new FormData();
    formData.append("file", files[0]);

    fetch(url, {
      method: "POST",
      body: formData
    })
      .then(res => res.json())
      .then(res => {
        bening = (res.bening * 100).toFixed(2);
        malignant = (res.malignant * 100).toFixed(2);
        console.log(res);
      });
  };
</script>

<style>
  div {
    max-width: 600px;
    margin: 0 auto;
    display: grid;
    align-items: center;
    justify-content: center;
    grid-gap: 20px;
  }

  img {
    max-width: 90%;
    max-height: 200px;
    margin: 0 auto;
  }

  h1,
  p {
    margin: 0;
    text-align: center;
  }

  input,
  p {
    box-sizing: border-box;
    padding: 0px 10px;
  }

  button {
    width: 100px;
    margin: 0 auto;
    background-color: rgb(46, 231, 169);
    border: none;
  }
</style>

<div>
  <h1>Skin Cancer</h1>
  <p>Select an image of a skin mole to get a prediction.</p>
  <input
    id="fileinput"
    type="file"
    accept="image/*"
    bind:files
    on:change={draw} />

  <img src="" alt="" id="image" />

  {#if files}
    <button on:click={predict} id="button">PREDICT</button>
  {/if}

  {#if bening}
    <p>Bening: {bening} %</p>
    <p>Malignant: {malignant} %</p>
  {/if}
</div>
