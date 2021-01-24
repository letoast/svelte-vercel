<script lang="ts">
  import { onMount } from "svelte";
  export let date: date;

  onMount(async () => {
    const res = await fetch("/api/date");
    const newDate = await res.text();
    date = newDate;
  });
</script>

<main>
  <h1>Svelte + Node.js API</h1>
  <h2>The date according to Node.js is:</h2>
  <p>{date ? date : 'Loading date...'}</p>
</main>
