<script lang="ts">
  // props
  export let label: string;
  export let value: number;
  export let id: string;
</script>

<div class="range">
  <label for={id}>{label} {value.toFixed(1)} (m)</label>
  <input type="range" bind:value min="1" max="10" step="0.1" {id} />
</div>

<style>
  .range {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 1rem;
  }
</style>
