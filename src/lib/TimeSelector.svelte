<script>
    // @ts-nocheck

    import { createEventDispatcher } from 'svelte'

    export let start
    export let end

    const dispatch = createEventDispatcher()
</script>

<div>
<div>
    <label for="start">Start Time:</label>
    <input
        step="3600000"
        type="time"
        id="start"
        name="start"
        value={start}
        on:change={e => dispatch('start', e.target?.value)}
    />
</div>
<br/>
<div>
    <label for="end">End Time:</label>
    <input
        step="3600000"
        type="time"
        id="end"
        name="end"
        value={end}
        on:change={e => dispatch('end', e.target?.value)}
    />
</div>
</div>
