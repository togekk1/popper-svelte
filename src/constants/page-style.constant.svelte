<script lang="ts" context="module">
  export const PAGE_STYLE = {
    POPPER_PLACEMENT: {
      top: "flex-col !mb-4",
      bottom: "flex-col !mt-4",
      left: "!mr-4",
      right: "!ml-4",
    } as { [key: string]: svelte.JSX.ClassName },
    ARROW_PLACEMENT: {
      top: "bottom-[-8px] !rotate-180",
      bottom: "top-[-8px]",
      left: "right-[-8px] !rotate-90",
      right: "left-[-8px] !-rotate-90",
    } as { [key: string]: svelte.JSX.ClassName },
  };
</script>
