<script lang="ts">
  import { createPopper } from "@popperjs/core";
  import type { Placement, ModifierArguments } from "@popperjs/core";
  import { PAGE_STYLE } from "./constants/page-style.constant.svelte";
  import PopperInner from "popper-svelte/src/popper-inner.svelte";
  import { createEventDispatcher } from "svelte";

  export let reference: HTMLElement;
  export let id: string | undefined = undefined;
  export let container_class: svelte.JSX.ClassName = "";
  export let popper_show: boolean | undefined = undefined;
  export let popper_class_show: 1 | undefined = undefined;
  export let placement: Placement;
  export let has_arrow: boolean | undefined = undefined;
  export let no_container: boolean | undefined = undefined;
  export let current_placement: Placement | undefined = undefined;
  export let fixed: boolean | undefined = undefined;

  let popper_class: svelte.JSX.ClassName = "";
  export { popper_class as class };

  let popper: HTMLDivElement;
  let popper_arrow: HTMLElement;
  let show_local: boolean;

  const dispatch = createEventDispatcher();

  export const toggle = (show?: boolean) => {
    popper_show = show ?? !popper_show;

    if (popper_show) {
      setTimeout(() => {
        createPopper(reference, popper, {
          placement,
          ...(fixed ? { strategy: "fixed" } : {}),

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
      }, 0);
    } else {
      popper_class_show = undefined;
    }
  };
</script>

{#if popper_show}
  <div
    {id}
    bind:this={popper}
    class="fixed {popper_class} {has_arrow
      ? PAGE_STYLE.POPPER_PLACEMENT[current_placement]
      : ''} !z-[99999]"
    class:opacity-0={!popper_class_show}
    on:mouseenter={() => {
      dispatch("mouseenter");
    }}
    on:mouseleave={() => {
      dispatch("mouseleave");
    }}
  >
    <PopperInner {...$$props}>
      <slot />
    </PopperInner>
  </div>
{/if}
