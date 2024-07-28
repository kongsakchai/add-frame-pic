<script lang="ts">
  import { onMount } from "svelte";
  import "./app.css";
  import InputRange from "./components/InputRange.svelte";
  import { saveAs } from "file-saver";
  import { toPng } from "html-to-image";

  let url = "";
  let value = 0;
  let x = 0;
  let y = 0;

  let inputFile: HTMLInputElement;
  let imageDom: HTMLElement;

  const onFileChange = (e: Event) => {
    console.log("onFileChange");
    const input = e.target as HTMLInputElement;
    if (!input.files) return;

    const file = input.files[0];
    url = URL.createObjectURL(file);

  };

  const saveFile = () => {
    let w=1024;
    let h=1024;
    const image = new Image()
    image.onload = () => {
      w = image.width;
      h = image.height;

    }

    toPng(imageDom,{
      "width":w,
      "height":h,
    })
      .then((blob) => {
        saveAs(blob, `saved-${Date.now()}.png`);
      })
      .catch(function (error) {
        console.error("oops, something went wrong!", error);
      });
  };

  const saveBorder = () => {
    localStorage.setItem("add-pic-fame.border", value.toString());
  };

  onMount(() => {
    const border = localStorage.getItem("add-pic-fame.border");
    if (border) {
      value = parseInt(border);
    } else {
      value = 0;
    }
  });
</script>

<main class=" min-h-[100vh] flex flex-col place-content-center gap-6 p-6">
  <section class="mx-auto">
    <div bind:this={imageDom} class=" aspect-square md:w-[600px] w-[300px] bg-white" style="padding: {value}px;">
      <div class="bg-neutral-600 w-full h-full overflow-hidden">
        <img src={url} alt="img" class=" object-cover w-full h-full" style="object-position: {x}% {y}%;" />
      </div>
    </div>
  </section>

  <input bind:this={inputFile} type="file" accept="image/*" hidden on:change={onFileChange} />

  <section class="mx-auto w-[300px] flex flex-col gap-4">
    <InputRange bind:value title="Border" onChange={() => saveBorder()} />
    <InputRange bind:value={x} title="X" />
    <InputRange bind:value={y} title="Y" />
    <button class=" w-[300px] h-[60px] bg-blue-300 font-semibold" on:click={() => inputFile.click()}>
      Upload Image
    </button>
    <button class=" w-[300px] h-[60px] bg-blue-300 font-semibold" on:click={saveFile}> Save Image </button>
  </section>
</main>
