<script lang="ts">
  import { createPopper } from "@popperjs/core";
  import type { Placement, ModifierArguments } from "@popperjs/core";
  import { PAGE_STYLE } from "./constants/page-style.constant.svelte";
  import { createEventDispatcher } from "svelte";

  export let reference: HTMLElement;
  export let id: string | undefined = undefined;
  export let popper_class: svelte.JSX.ClassName = "";
  export let container_class: svelte.JSX.ClassName = "";
  export let popper_show: boolean | undefined = undefined;
  export let popper_class_show: 1 | undefined = undefined;
  export let placement: Placement;
  export let has_arrow: boolean | undefined = undefined;
  export let no_container: boolean | undefined = undefined;
  export let current_placement: Placement | undefined = undefined;

  let popper: HTMLDivElement;
  let popper_arrow: HTMLElement;
  let show_local: boolean;

  const dispatch = createEventDispatcher();

  export const toggle = (show?: boolean) => {
    popper_show = show ?? !popper_show;

    if (popper_show) {
      document.body.append(popper);
      show_local = true;
      setTimeout(() => {
        createPopper(reference, popper, {
          placement,

          modifiers: [
            {
              name: "arrow",
              options: {
                element: popper_arrow,
              },
              fn({ state }: ModifierArguments<any>) {
                current_placement = state.placement;
              },
            },
          ],
        });
        setTimeout(() => {
          popper_class_show = 1;
        }, 0);
      }, 100);
    } else {
      document.body.removeChild(popper);
      show_local = false;
      popper_class_show = undefined;
    }
  };
</script>

<div
  {id}
  bind:this={popper}
  class="{popper_class} {has_arrow
    ? PAGE_STYLE.POPPER_PLACEMENT[current_placement]
    : ''} z-[9999]"
  class:opacity-0={!popper_class_show}
  class:hidden={!show_local}
  on:mouseenter={() => {
    dispatch("mouseenter");
  }}
  on:mouseleave={() => {
    dispatch("mouseleave");
  }}
>
  {#if popper_show}
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
      <div class="relative {container_class}"><slot /></div>
    {/if}
  {/if}
</div>
