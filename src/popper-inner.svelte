<script lang="ts">
  import { PAGE_STYLE } from "./constants/page-style.constant.svelte";
  import type { Placement } from "@popperjs/core";

  export let has_arrow: boolean | undefined = undefined;
  export let popper_arrow: HTMLElement;
  export let container_class: svelte.JSX.ClassName = "";
  export let no_container: boolean | undefined = undefined;
  export let current_placement: Placement | undefined = undefined;
</script>

{#if has_arrow}
  <div
    id="arrow"
    class="self-center border-x-[6px] border-x-transparent border-b-[8px] border-b-white {PAGE_STYLE
      .ARROW_PLACEMENT[current_placement] ?? ''}"
    bind:this={popper_arrow}
  />
{/if}
{#if no_container}
  <slot />
{:else}
  <div class="relative z-50 w-full {container_class}"><slot /></div>
{/if}
