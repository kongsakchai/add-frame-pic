<script lang="ts">
  export let value = 0;
  export let title = "Range";
  export let min = 0;
  export let max = 100;
  export let step = 1;
  export let onChange = () => {};
</script>

<section class="flex items-center gap-4 w-full">
  <label class="w-[90px]" for="range" on:input={onChange}>{title}</label>
  <input class="w-full" type="range" id="range" bind:value {min} {max} {step} />
  <input class="w-[50px] text-center bg-transparent" type="number" bind:value on:change={onChange} />
</section>
