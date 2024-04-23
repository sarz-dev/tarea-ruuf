<script lang="ts">
  export let rectWidth: number;
  export let rectHeight: number;
  export let horizontalCount: number;
  export let verticalCount: number;
  export let panelWidth: number;
  export let panelHeight: number;
</script>

<div style="width: {rectWidth}px; height: {rectHeight}px;" class="rectangle">
  {#each { length: horizontalCount } as _}
    {#each { length: verticalCount } as _}
      <div
        class="panel"
        style="width: {panelWidth}px; height: {panelHeight}px;"
      />
    {/each}
  {/each}
</div>

<style>
  .rectangle {
    flex-wrap: wrap;
    flex-direction: row;
    display: flex;
    justify-content: space-around;
    align-items: center;
    border-radius: 10px;
  }
  .panel {
    background-color: #222222;
    border: 1px solid orange;
    box-sizing: border-box;
    border-radius: 10px;
  }
</style>
