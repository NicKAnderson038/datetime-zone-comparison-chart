<script>
    // @ts-nocheck

    // import svelteLogo from './assets/svelte.svg'
    // import viteLogo from '/vite.svg'
    import moment from 'moment'
    import TimeSelector from './lib/TimeSelector.svelte'
    import Select from './lib/Select.svelte'
    import DatetimeChart from './lib/DatetimeChart.svelte'

    const currentTimezone = Intl.DateTimeFormat()
        .resolvedOptions()
        .timeZone.replace(/.*\//, '')
        .replaceAll('_', ' ')

    const myTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone

    let start = '09:00'
    let end = '17:00'
    export let timeZoneSelection1 = myTimezone
    export let timeZoneSelection2 = 'America/Chicago'

    function startSelect(e) {
        start = e.detail
    }

    function endSelect(e) {
        end = e.detail
    }

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
    <TimeSelector {start} {end} on:start={startSelect} on:end={endSelect} />
    <h3>
        <span class="dot" />My Current Location & Day: {currentTimezone} - {moment().format(
            'dddd'
        )}
    </h3>
    <!-- <div class="card">
        <Select on:timezone={myTimeZone} selected={myTimezone} myLocation />
    </div> -->
    <div class="card">
        <Select on:timezone={companyTimeZone} selected="America/Chicago" />
    </div>

    <DatetimeChart
        timeZone1={timeZoneSelection1}
        timeZone2={timeZoneSelection2}
        {start}
        {end}
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
        background-color: #0080e2;
        border-radius: 50%;
        display: inline-block;
        margin-right: 6px;
    }
</style>
