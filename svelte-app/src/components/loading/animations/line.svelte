<script lang="ts">
  export let width = '200px',
    height = '4px',
    classes = '';
</script>

<div class="wrapper {classes}" style="width: {width}; height: {height};">
  <div data-test-id="loader-bar">
    {#each Array(7) as _}
      <div
        class="bg-violet-300 shadow-[0_0_16px_var(--tw-shadow)] shadow-violet-300"
      />
    {/each}
  </div>
</div>

<style lang="scss">
  .wrapper {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;

    > div {
      height: 100%;
      display: flex;
      transform: translateZ(0);

      div {
        flex: 1;
        animation: pulse 700ms infinite alternate
          cubic-bezier(0.16, -0.05, 0.35, 1.02);

        @for $i from 1 through 8 {
          &:nth-child(#{$i}) {
            animation-delay: -(calc(700ms / 8) * (8 - $i));
          }
        }
      }
    }
  }

  @keyframes pulse {
    100% {
      background: transparent;
      flex: 10;
      box-shadow: 0 0 0 rgba(196 181 253 / 0.25);
    }
  }
</style>
