<script>
    // @ts-nocheck

    // import svelteLogo from './assets/svelte.svg'
    // import viteLogo from '/vite.svg'
    import Select from './lib/Select.svelte'
    import DatetimeChart from './lib/DatetimeChart.svelte'

    const currentTimezone = Intl.DateTimeFormat()
        .resolvedOptions()
        .timeZone.replace(/.*\//, '')
        .replaceAll('_', ' ')

    const myTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone

    export let timeZoneSelection1 = myTimezone
    export let timeZoneSelection2 = 'America/Chicago'

    function myTimeZone(e) {
        timeZoneSelection1 = e.detail
    }

    function companyTimeZone(e) {
        timeZoneSelection2 = e.detail
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
    <h3><span class="dot" />My Current Location: {currentTimezone}</h3>
    <!-- <div class="card">
        <Select on:timezone={myTimeZone} selected={myTimezone} myLocation />
    </div> -->
    <div class="card">
        <Select on:timezone={companyTimeZone} selected="America/Chicago" />
    </div>

    <DatetimeChart
        timeZone1={timeZoneSelection1}
        timeZone2={timeZoneSelection2}
    />
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
        background-color: #008FFB;
        border-radius: 50%;
        display: inline-block;
        margin-right: 6px;
    }
</style>
