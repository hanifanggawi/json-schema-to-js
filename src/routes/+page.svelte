<script lang="ts">
    import '../app.css'

    let input: string
    let output: string

    function transformJSONSchema() {
        console.log('hubla')
        try {
            const jsonString = JSON.parse(input);
        
            console.log(jsonString)
            delete jsonString['x-apidog-orders']        
            // The replacer function that will replace double quotes with single quotes
            const result = JSON.stringify(jsonString, (_key, value) => typeof value === 'string' ? `'${value}'` : value, 2)
                .replace(/"([^"]+)"(?=:)/g, '$1')
                .replace(/""/g, '')
                .replaceAll(`"`, ``)
                .replace(/\[[^\]]*]/g, function(match) {
                    return match.replace(/\s/g, '');
                })
            
            output = result
        }
        catch (error) {
            console.error(error)
            output = 'Malformed Input'
        }
    }

    function copyOutput() {
        navigator.clipboard.writeText(output)
    }
</script>

<div class="row-container">
    <h1>Json Schema to Validator Schema</h1>
    <form on:submit|preventDefault={transformJSONSchema} >
        <div class="input-boxes">
            <textarea name="json" id="" cols="60" rows="30" bind:value={input}></textarea>
            <textarea name="javascript" id="" cols="60" rows="30" bind:value={output}></textarea>
        </div>
        <div class="buttons">
            <button type="submit">Convert</button>
            {#if output}
                <button on:click={copyOutput}>Copy to Clipboard</button>
            {/if}
        </div>
    </form>
</div>

<style>
    .row-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        row-gap: 1em;
    }

    .buttons {
        display: flex;
        justify-content: space-evenly;
    }
</style>
