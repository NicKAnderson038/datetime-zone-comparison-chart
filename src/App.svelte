<script>
    // @ts-nocheck

    // import svelteLogo from './assets/svelte.svg'
    // import viteLogo from '/vite.svg'
    import Select from './lib/Select.svelte'
    import DatetimeChart from './lib/DatetimeChart.svelte'

    const myTimezone = Intl.DateTimeFormat()
        .resolvedOptions()
        .timeZone.replace(/.*\//, '')
        .replaceAll('_', ' ')

    export let timeZoneSelection
    function timeZone(e) {
        timeZoneSelection = e.detail
    }
</script>

<main>
    <!-- <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div> -->
    <h1>Time Zone Comparison</h1>
    <h3><span class="dot" />My Location: {myTimezone}</h3>
    <div class="card">
        <Select on:timezone={timeZone} />
    </div>

    <DatetimeChart timeZone={timeZoneSelection} />
</main>

<style>
    h1 {
        border-bottom: 2px solid;
    }

    h3 {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .dot {
        height: 25px;
        width: 25px;
        background-color: #008ffb;
        border-radius: 50%;
        display: inline-block;
        margin-right: 6px;
    }
</style>
