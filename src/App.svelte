<script>
  import CircleElements from './CircleElements.svelte'
  import { onMount } from 'svelte'
  import { gender } from './data.js'

 let genderCounts = {};

  onMount(() => {
    genderCounts = calculateGenderCounts(gender);
  });

  function calculateGenderCounts(data) {
  const counts = {};

  data.forEach(item => {
    const year = item.year;
    const gender = item.author_gender;

    if (!counts[year]) {
      counts[year] = {};
    }

    if (!counts[year][gender]) {
      counts[year][gender] = 0;
    }

    counts[year][gender]++;
  });

  return counts;
}

let selectedBar = null;

let selectedBarElement;

function handleBarClick(event, year, gender, count) {
  selectedBar = { year, gender, count };

  const barSegment = event.currentTarget;
  const rect = barSegment.getBoundingClientRect();

  selectedBarPosition = {
    top: rect.top + window.scrollY + rect.height + 5, // Adjust as needed
    left: rect.left + window.scrollX + rect.width / 2, // Adjust as needed
  };
}

let selectedBarPosition = { top: 0, left: 0 };

  function clearSelectedBar() {
    selectedBar = null;
  }
  
</script>

<div class="horizontal-bar-chart">
  {#each Object.entries(genderCounts) as [year, counts]}
    <div class="bar-container">
      <div class="year-label">{year}</div>
      <div class="bar">
        {#each Object.entries(counts) as [gender, count]}
          <div
            class="bar-segment"
            style="width: {count * 10}px; background-color: {
              gender === 'Male' ? '#990000' :
              gender === 'Female' ? '#FEBE00' :
              gender === 'Both' ? '#5B7742' :
              gender === 'Undisclosed' ? '#0081DD' :
              'gray'
            }"
            on:click={() => handleBarClick(event, year, gender, count)}
            on:keydown={(event) => {
              if (event.key === 'Enter' || event.key === 'Space') {
                handleBarClick(event, year, gender, count);
              }
            }}
            role="button"
            tabindex="0"
            on:mouseleave={clearSelectedBar}
          >
          </div>
        {/each}
      </div>
    </div>
  {/each}
</div>

{#if selectedBar !== null}
  <div
    class="popup"
    style={`top: ${selectedBarPosition.top}px; left: ${selectedBarPosition.left}px;`}
  >
    {selectedBar.gender}: {selectedBar.count}
  </div>
{/if}

<style>
  .horizontal-bar-chart {
    margin-bottom: 20px;
    margin-top: 50px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .bar-container {
    margin: 10px;
    display: flex;
    align-items: center;
  }

  .year-label {
    font-size: 14px;
    margin-right: 10px;
  }

  .bar {
    display: flex;
    align-items: center;
  }

  .bar-segment {
    height: 25px;
    margin-right: 3px;
    display: flex;
    align-items: center;
    color: white;
    font-size: 12px;
    padding: 0 5px;
  }

    .popup {
    position: absolute;
    top: 0;
    left: 0;
    background-color: white;
    border: 1px solid #ccc;
    padding: 5px;
    z-index: 100;
  }

</style>
