<script>
  import Highlight, { LineNumbers } from "svelte-highlight";
  import json from "svelte-highlight/languages/json";
  import "highlight.js/styles/github.css";

  let method = "GET";
  let url;
  let result = "";
  let body = "";
  let status = "";
  async function SubmitURL() {
     console.log('Method:', method);
  console.log('URL:', url);
  console.log('Body:', body);
  console.log('Body JSON:', JSON.stringify(body));
    let options = {
      method: method,
      headers: {
    'Content-Type': 'application/json'
      },
    };

    if (body) {
    // If body is already a string, don't stringify again
    if (typeof body === 'string') {
      options.body = body;
    } else {
      options.body = JSON.stringify(body);
    }
  }

    const response = await fetch(url, options);
    status = String(response.status);
    const data = await response.json();
    result = JSON.stringify(data, null, 2);
  }
</script>

<main>
  
  <div class="pl-2 pr-2 flow flex-col">
    <div class="flex gap-2 mt-4">
      <select
        bind:value={method}
        class="select select-info select-sm flex-none w-32"
      >
        <option>GET</option>
        <option>POST</option>
        <option>PUT</option>
        <option>PATCH</option>
        <option>DELETE</option>
      </select>
      <div class="grow w-3/4">
        <label class="input input-sm input-bordered flex items-center gap-2">
          <input type="text" class="grow" placeholder="URL" bind:value={url} />
        </label>
      </div>
      <button
        class="btn btn-neutral flex-initial w-24 btn-sm"
        on:click={SubmitURL}>Submit</button
      >
    </div>

    <label class="form-control font-sans">
      <div class="label">
        <span class="label-text font-bold">Body :</span>
      </div>

      <textarea   class="textarea textarea-bordered h-32 font-mono leading-tight"  bind:value={body}></textarea>
    </label>

    <!-- svelte-ignore a11y-label-has-associated-control -->
    <label class="form-control font-sans">
      <div class="label">
        <span class="label-text font-bold">Result :</span>
        <span class="text-xs text-red-600">Status : {status}</span>
      </div>
      <div class="border rounded-sm overflow-auto h-72 text-xs">
        <Highlight language={json} code={result} let:highlighted>
          <LineNumbers {highlighted} hideBorder wrapLines />
        </Highlight>
      </div>
    </label>
  </div>
</main>
