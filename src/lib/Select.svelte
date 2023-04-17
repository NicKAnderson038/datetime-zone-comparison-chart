<script>
// @ts-nocheck

    import { createEventDispatcher } from "svelte"

    const dispatch = createEventDispatcher();
    // @ts-ignore
    const timeZoneList = Intl.supportedValuesOf('timeZone');

    let selected = 'America/Chicago'

</script>

<label><span class="dot" />Compare Location</label>
<select value={selected} on:change={(e) => dispatch('timezone', e.target?.value)}>
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
        background-color: #00E396;
        border-radius: 50%;
        display: inline-block;
        margin-right: 6px;
        top: 6px;
        position: relative;
    }
</style>
