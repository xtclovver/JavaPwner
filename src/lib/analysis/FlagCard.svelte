<script lang="ts">
  import { Divider, Button, easeEmphasized, sharedAxisTransition } from "m3-svelte";
  import { slide } from "svelte/transition";
  import type { Flag } from "./createAnalysis";
  import { view } from "$lib/state";

  export let name: string;
  export let flag: Flag;
  let expandedFiles = false;
</script>

<div
  class="bg-background flex flex-col items-center rounded-xl p-4"
  in:sharedAxisTransition={{ direction: "Z", leaving: false }}
>
  <h2 class="m3-font-headline-small">{name}</h2>
  {#if expandedFiles || flag.matches.length == 1}
    <div class="w-full" transition:slide={{ easing: easeEmphasized }}>
      {#each flag.matches as file}
        <button
          class="underline-hover text-primary mt-2 block truncate font-mono underline"
          on:click={() => {
            $view = { tab: "browser", editorFile: file, editorFind: flag.initialFind };
          }}
          title={file}
        >
          {file}
        </button>
      {/each}
    </div>
  {:else}
    <p class="mt-2" transition:slide={{ easing: easeEmphasized }}>
      {flag.matches.length}
      {flag.matches.length == 1 ? "file" : "files"}
    </p>
  {/if}
  {#if flag.matches.length > 1}
    <div class="mt-auto w-full py-4">
      <Divider />
    </div>
    <div class="flex justify-center gap-2">
      <Button variant="tonal" click={() => (expandedFiles = !expandedFiles)}>
        {expandedFiles ? "Collapse" : "Expand"} files
      </Button>
      {#if flag.link}
        <Button type="text" href={flag.link} target="_blank">Read about it</Button>
      {/if}
    </div>
  {:else if flag.link}
    <div class="mt-auto flex justify-center pt-4">
      <Button type="text" href={flag.link} target="_blank">Read about it</Button>
    </div>
  {/if}
</div>
