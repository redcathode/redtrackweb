<script>
    var submission = new Array();
    submission[0] = {
        fieldname: "mood",
        fieldtype: "numerical-rating",
        fieldval: "0",
    };

    async function submitPost() {
        let post = JSON.stringify(submission);
        const res = await fetch("/api/v1/posts/submit", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: post,
        });
        console.log(res);
        submission = [];
    }
    function addFeature(fieldname, fieldtype) {
        let val = "";
        if (fieldtype == "numerical-rating") {
            val = "0";
        }
        submission = [
            ...submission,
            { fieldname: fieldname, fieldtype: fieldtype, fieldval: val },
        ];
    }
    function removeFeature(item) {
        submission = submission.filter((i) => i !== item);
    }
</script>

<div id="post-submit-interface">
    <h3>submit</h3>
    <div id="add-buttons">
        <button class="add" style="margin-left: 0px;" on:click={() => addFeature("mood", "numerical-rating")}>add feature</button>
        <button class="add" on:click={() => addFeature("notes", "text-notes")}>add notes</button>
        <button class="add" style="margin-right: 0px;" on:click={() => addFeature("tripped", "event")}>add event</button>
    </div>
    <div id="submit-values">
        {#each submission as item}
            <div class="submission-item">
                <div class="first-row">
                    <input type="text" class="fieldname-input" style="padding: 0; padding-left: 5px;" bind:value={item.fieldname} />
                    <button class="delete-btn" on:click={() => removeFeature(item)}>-</button>
                </div>
                <div class="second-row">
                    {#if item.fieldtype == "numerical-rating"}
                        <!-- TODO: something something js -->
                        <input type="range" min="-5" max="5" step="0.5" bind:value={item.fieldval} />
                        <input type="text" class="small-num" bind:value={item.fieldval} />
                    {:else if item.fieldtype == "text-notes"}
                        <input type="text" bind:value={item.fieldval} />
                    {/if}
                    <select name="fieldtype" id="fieldtype" bind:value={item.fieldtype}>
                        <option value="numerical-rating">rating (num)</option>
                        <option value="text-notes">notes</option>
                        <option value="event">event</option>
                    </select>
                </div>
            </div>
        {/each}
    </div>
    <button id="submitPost" on:click={submitPost}>submit</button>
</div>

<style>
    #submitPost {
        width: 100%;
        
    }
    #add-buttons {
        display: flex;
        width: 100%;
    }

    .add {
        width: 100%;
    }

    #fieldtype {
        margin-right: 5px;
    }

    .submission-item {
        background-color: var(--surface2);
        display: flex;
        flex-direction: column;
        box-sizing: border-box;
        padding: 10px;
        margin-top: 5px;
        margin-bottom: 5px;
        border-radius: 5px;
        padding-left: 5px;
        padding-right: 0;
    }
    #post-submit-interface {
        background-color: var(--surface1);
        display: flex;
        flex-direction: column;
        align-items: center;
        box-sizing: border-box;
        padding: 10px;
        border-radius: 5px;
        margin: 5px;
    }
    .first-row {
        display: flex;
        margin-left: 0px;
    }
    .fieldname-input {
        width: 100%;
        float: left;
    }
    .second-row {
        display: flex;
        align-items: center;
        padding: 5px;
        margin-top: 5px;
        margin-left: 0px;
    }
    .delete-btn {
        float: right;
        margin: 0;
        margin-left: 5px;
        margin-right: 10px;
        background-color: var(--overlay0);
    }
    .small-num {
        width: 40px;
    }
    select {
        background-color: var(--overlay0);
        color: var(--text);
        border: none;
        height: 40px;
        border-radius: 5px;
        margin-right: 0;
        margin-left: 5px;
    }
    input {
        background-color: var(--overlay0);
        color: var(--text);
        border: none;
        margin-left: 5px;
        margin-right: 5px;
        padding: 0;
        padding-left: 5px;
        height: 40px;
        border-radius: 5px;
    }
    button {
        background-color: var(--surface2);
        color: var(--text);
        border: none;
        margin: 5px;
        min-width: 40px;
        height: 40px;
        border-radius: 5px;
    }
    h3 {
        color: var(--text);
        margin: 5px;
    }
</style>