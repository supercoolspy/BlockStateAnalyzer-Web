<script lang="ts">
    let data: Map<String, String> = new Map<String, String>();
    let blockState = ""
    let error = "Please wait loading block states ..."
    async function fetchJson(url: string): Promise<Map<string, string>> {
        const response = await fetch(url);
        const json = await response.json();

        const dataMap = new Map<string, string>();

        // Iterate over the JSON object and add key-value pairs to the map
        Object.entries(json).forEach(([key, value]) => {
            dataMap.set(key, String(value));
        });
        return dataMap;
    }

    function search() {
        let inputText = (document.getElementById("input") as HTMLInputElement).value
        const input = inputText as number
        console.log("input: " + input)
        if (isNaN(input)) {
            error = "Please enter a valid number"
            return
        }
        error = ""
        blockState = data.get(inputText) ? data.get(inputText) : "Not found";
    }

    fetchJson("/blockstates.json")
        .then((dataMap) => {
            // Use the map with key-value pairs
            data = dataMap;
            document.getElementById("search").removeAttribute("hidden");
            error = "";
        })
        .catch((err) => {
            error = "Error loading block states: " + err;
        });
</script>
<h1>Network Block State Search</h1>
<p id="error">{error}</p>
<div id="search" hidden="hidden">
    <input id="input" type="text" placeholder="ID" />
    <button type="button" on:click={() => search()}>Search</button>
    <p id="response">{blockState}</p>
</div>
<style>
    #error {
        color: red;
    }
</style>
