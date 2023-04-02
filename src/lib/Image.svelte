<script>
  let imageFile = null;
  let dom = null;

  function handleDrop(event) {
    event.preventDefault();
    const file = event.dataTransfer.files[0];
    if (file.type.startsWith("image/")) {
      imageFile = file;
    } else {
      alert("Please drop an image file.");
    }
  }

  function handleUploadDom(node) {
    dom = node;
    return {
      destroy() {
        dom = null;
      },
    };
  }

  function handleFileInput(event) {
    const file = event.target.files[0];
    if (file.type.startsWith("image/")) {
      imageFile = file;
      event.target.value = ''
    } else {
      alert("Please select an image file.");
    }
  }

  function handleUpload(event) {
    console.log("click--->");
    if (dom) {
      // perform upload logic here using imageFile
      dom.click();
    }
  }


  async function handlePaste(event) {
    const items = event.clipboardData.items;
    for (let i = 0; i < items.length; i++) {
      const item = items[i];
      if (item.type.startsWith("image/")) {
        console.log('handle paste')
        imageFile = item.getAsFile();
        // perform upload logic here using formData
      }
    }
  }
</script>

<svelte:window on:paste={handlePaste}/>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div
  on:dragover={(event) => event.preventDefault()}
  on:drop={handleDrop}
  class="image"
>
  {#if imageFile}
    <!-- svelte-ignore a11y-img-redundant-alt -->
    <img
      src={URL.createObjectURL(imageFile)}
      alt="Dropped image"
      style="max-width: 100%; max-height: 200px; margin-bottom: 1rem;"
    />
  {:else}
    <p class="placeholder">Drop an image file here or paste an image from clipboard</p>
  {/if}
</div>
<div class="action">
  <button on:click={handleUpload}>Select a image</button>
  <button on:click={() => {imageFile = null}}>Clear image</button>
</div>
<input
  type="file"
  accept="image/*"
  use:handleUploadDom
  on:change={handleFileInput}
/>

<style>
  .image {
    border: 2px dashed gray;
    padding:1rem;
    cursor: pointer;
  }
  .placeholder {
    padding: 2rem 0 ;
  }
  .action {
    margin-top: 10px;
  }
  .action button:last-child {
    background: #dedede;
  }
  input[type="file"] {
    visibility: hidden;
    position: absolute;
    width: 1px;
    height: 1px;
    left: 0;
  }
</style>
