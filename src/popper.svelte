<script lang="ts">
  import { createPopper } from "@popperjs/core";
  import type { Placement, ModifierArguments } from "@popperjs/core";
  import { PAGE_STYLE } from "./constants/page-style.constant.svelte";

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
  export let insert_into_body: boolean | undefined = undefined;

  let popper: HTMLDivElement;
  let popper_arrow: HTMLElement;
  let show_local: boolean;

  export const toggle = (show?: boolean) => {
    popper_show = show ?? !popper_show;

    if (popper_show) {
      if (insert_into_body) {
        document.body.append(popper);
        show_local = true;
      }
      setTimeout(
        () => {
          createPopper(reference, popper, {
            placement,
            strategy: "fixed",

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
        },
        insert_into_body ? 100 : 0
      );
    } else {
      if (insert_into_body) {
        document.body.removeChild(popper);
        show_local = false;
      }
      popper_class_show = undefined;
    }
  };
</script>

{#if popper_show}
  <div
    {id}
    bind:this={popper}
    class="{popper_class} {has_arrow
      ? PAGE_STYLE.POPPER_PLACEMENT[current_placement]
      : ''} {insert_into_body && !show_local ? 'hidden' : ''}"
    class:opacity-0={!popper_class_show}
  >
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
      <div class="relative z-50 {container_class}"><slot /></div>
    {/if}
  </div>
{/if}
