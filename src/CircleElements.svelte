<script>
  export let dots;
</script>

<div class="graph">
  <div class="title">
    <svg width="1000" height="100">
      {#each dots as dot}
        <div class={`dot ${dot.color}`} style={`transform: translate(${dot.left - 10}px, ${dot.top - 10}px);`}>
          <span class="dot-text">{dot.text}</span>
        </div>
      {/each}
    </svg>
  </div>
</div>

<style>
  .graph {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100px;
    width: 500px;
  }

  .dot-text {
    margin-left: 30px;
  }

  .dot {
    display: inline-block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    position: relative; /* Use relative positioning to position the dot */
  }

  .red {
    background-color: #990000;
  }

  .green {
    background-color: #5B7742;
  }

  .yellow {
    background-color: #FEBE00;
  }

  .blue {
    background-color: #0081DD;
  }
</style>
