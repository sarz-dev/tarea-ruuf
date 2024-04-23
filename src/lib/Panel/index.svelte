<script lang="ts">
  import Range from "./components/Range.svelte";
  import SubRect from "./components/SubRect.svelte";

  const MULTIPLIER = 100;
  let width = 5;
  let height = 3;
  let widthPanel = 1;
  let heightPanel = 2;

  // let width = 8;
  // let height = 1.9;
  // let widthPanel = 1.4;
  // let heightPanel = 1;

  $: widthFactor = Math.floor(width / widthPanel);
  $: heightFactor = Math.floor(height / heightPanel);

  $: auxWidthFactor = Math.floor(width / heightPanel);
  $: auxHeightFactor = Math.floor(height / widthPanel);

  $: rotatePanel = widthFactor * heightPanel < auxWidthFactor * auxHeightFactor;
  $: finalWidthFactor = rotatePanel ? auxWidthFactor : widthFactor;
  $: finalHeightFactor = rotatePanel ? auxHeightFactor : heightFactor;
  $: finalPanelWidth = rotatePanel ? heightPanel : widthPanel;
  $: finalPanelHeight = rotatePanel ? widthPanel : heightPanel;

  $: bottomRestHeight = height - finalHeightFactor * finalPanelHeight;
  $: bottomRestWidth = width;
  $: widthBottomFactor = Math.floor(bottomRestWidth / finalPanelHeight);
  $: heightBottomFactor = Math.floor(bottomRestHeight / finalPanelWidth);

  $: rightRestHeight =
    height -
    heightBottomFactor -
    (height - finalHeightFactor * finalPanelHeight);
  $: rightRestWidth = width - finalWidthFactor * finalPanelWidth;
  $: widthRightFactor = Math.floor(rightRestWidth / finalPanelHeight);
  $: heightRightFactor = Math.floor(rightRestHeight / finalPanelWidth);

  $: countRects =
    finalWidthFactor * finalHeightFactor +
    widthRightFactor * heightRightFactor +
    widthBottomFactor * heightBottomFactor;

  $: hasRightRects = widthRightFactor * heightRightFactor > 0;
  $: hasBottomRects = widthBottomFactor * heightBottomFactor > 0;

  $: parentWidth = MULTIPLIER * width;
  $: parentHeight = MULTIPLIER * height;
  $: mainWidth = MULTIPLIER * finalWidthFactor * finalPanelWidth;
  $: mainHeight = MULTIPLIER * finalHeightFactor * finalPanelHeight;
  $: rightWidth = MULTIPLIER * rightRestWidth;
  $: rightHeight = MULTIPLIER * rightRestHeight;
  $: bottomWidth = MULTIPLIER * bottomRestWidth;
  $: bottomHeight = MULTIPLIER * bottomRestHeight;
  $: panelWidth = MULTIPLIER * finalPanelWidth;
  $: panelHeight = MULTIPLIER * finalPanelHeight;
</script>

<div class="container">
  <div class="inputs-container">
    <Range label="Ancho Techo" bind:value={width} id="width" />
    <Range label="Alto Techo" bind:value={height} id="height" />
    <Range label="Ancho Panel" bind:value={widthPanel} id="widthPanel" />
    <Range label="Alto Panel" bind:value={heightPanel} id="heightPanel" />
  </div>
  <div>
    <p>{countRects} Panel{countRects !== 1 ? "es" : ""}</p>
  </div>
  <div
    style="width: {parentWidth}px; height: {parentHeight}px;"
    class="rectangle parent"
  >
    <SubRect
      rectWidth={mainWidth}
      rectHeight={mainHeight}
      horizontalCount={finalHeightFactor > 0 ? finalHeightFactor : 0}
      verticalCount={finalWidthFactor > 0 ? finalWidthFactor : 0}
      {panelWidth}
      {panelHeight}
    />
    {#if hasRightRects}
      <SubRect
        rectWidth={rightWidth}
        rectHeight={rightHeight}
        horizontalCount={heightRightFactor > 0 ? heightRightFactor : 0}
        verticalCount={widthRightFactor > 0 ? widthRightFactor : 0}
        panelWidth={panelHeight}
        panelHeight={panelWidth}
      />
    {/if}
    {#if hasBottomRects}
      <SubRect
        rectWidth={bottomWidth}
        rectHeight={bottomHeight}
        horizontalCount={heightBottomFactor > 0 ? heightBottomFactor : 0}
        verticalCount={widthBottomFactor > 0 ? widthBottomFactor : 0}
        panelWidth={panelHeight}
        panelHeight={panelWidth}
      />
    {/if}
  </div>
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
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .inputs-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 2rem;
  }
  .parent {
    background-color: #4d2725;
  }
</style>
