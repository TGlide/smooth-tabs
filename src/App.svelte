<script lang="ts">
  import "@fontsource/inter";

  const tabs = ["Hey", "Whats Up", "Hola"];
  let selected = 0;
  let list: HTMLUListElement;

  $: style = (function getStyle(): string {
    const listRect = list?.getBoundingClientRect();
    const selRect = list
      ?.querySelector(`[id='${tabs[selected]}']`)
      ?.getBoundingClientRect();
    if (!listRect || !selRect) return "";

    const left = selRect.left - listRect.left;
    return `width: ${selRect.width}px;
      height: ${selRect.height}px;
      transform: translateX(${left - 1}px);
      left: 0;
      top: 0.125rem;
      transition: 400ms cubic-bezier(0.25, 1, 0.5, 1);`;
  })();

  $: console.log(style);
</script>

<main class="bg-purple-1  h-screen w-screen grid place-items-center">
  <ul
    class="flex gap-2 p-0.5 border border-solid border-white rounded-[8px] relative"
    bind:this={list}
  >
    <div id="bg" class="bg-white rounded-[6px] absolute z-0" {style} />
    {#each tabs as tab, i}
      <li id={tab} data-selected={i === selected}>
        <button on:click={() => (selected = i)}>
          {tab}
        </button>
      </li>
    {/each}
  </ul>
</main>

<style lang="postcss">
  main {
    background: linear-gradient(
      120deg,
      hsl(252, 83%, 89%),
      hsl(252, 83%, 82%, 70%)
    );
  }

  li {
    color: theme("colors.sand.9");
    cursor: pointer;
    position: relative;
    z-index: theme("zIndex.10");

    transition: color 250ms ease;

    &:hover,
    &[data-selected="true"] {
      color: theme("colors.sand.11");
    }

    button {
      padding: theme("spacing.2") theme("spacing.4");
    }
  }
</style>
