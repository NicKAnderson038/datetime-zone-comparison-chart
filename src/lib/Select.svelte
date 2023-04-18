<script>
    // @ts-nocheck

    import { createEventDispatcher } from 'svelte'

    const dispatch = createEventDispatcher()
    // @ts-ignore
    const timeZoneList = Intl.supportedValuesOf('timeZone')

    export let selected
    export let myLocation
</script>

{#if myLocation}
    <label for="timezone"><span class="dot dot-blue" />Company Location</label>
{:else}
    <label for="timezone"><span class="dot dot-green" />Future Location</label>
{/if}
<select
    id="timezone"
    name="timezone"
    value={selected}
    on:change={e => dispatch('timezone', e.target?.value)}
>
    {#each timeZoneList as question}
        <option value={question}>
            {question}
        </option>
    {/each}
</select>

<style>
    select {
        padding: 10px;
    }

    .dot {
        height: 25px;
        width: 25px;
        border-radius: 50%;
        display: inline-block;
        margin-right: 6px;
        top: 6px;
        position: relative;
    }

    .dot-blue {
        background-color: #008ffb;
    }

    .dot-green {
        background-color: #00e396;
    }
</style>
