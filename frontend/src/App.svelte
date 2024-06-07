<script>
  import Highlight, { LineNumbers } from "svelte-highlight";
  import json from "svelte-highlight/languages/json";
  import "highlight.js/styles/github.css";
  import { Quit } from "./lib/wailsjs/runtime/runtime";

  let method = "GET";
  let url;
  let result = "";
  let body = "";
  let status = "";
  async function SubmitURL() {
    let options = {
      method: method,
    };

    if (body) {
      options.body = JSON.stringify(body);
    }

    const response = await fetch(url, options);
    status = String(response.status);
    const data = await response.json();
    result = JSON.stringify(data, null, 2);
  }
</script>

<main>
  <div class="flex gap-2 mb-2 bg-slate-300 rounded-sm text-2xl">
    <span class="select-none">🐱‍👤</span>
    <span class="flex-1 text-center select-none" style="--wails-draggable:drag"
      >Postkid</span
    >
    <button on:click={() => Quit()} class="hover:bg-slate-400 btn-xs">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        fill="none"
        viewBox="0 0 24 24"
        stroke="red"
        ><path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        /></svg
      >
    </button>
  </div>
  <div class="pl-2 pr-2 flow flex-col">
    <div class="flex gap-2">
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

      <textarea class="textarea textarea-bordered" bind:value={body}></textarea>
    </label>

    <!-- svelte-ignore a11y-label-has-associated-control -->
    <label class="form-control font-sans">
      <div class="label">
        <span class="label-text font-bold">Result :</span>
        <span class="text-xs text-red-600">Status : {status}</span>
      </div>
      <div class="border rounded-sm overflow-auto h-80 text-xs">
        <Highlight language={json} code={result} let:highlighted>
          <LineNumbers {highlighted} hideBorder wrapLines />
        </Highlight>
      </div>
    </label>
  </div>
</main>
