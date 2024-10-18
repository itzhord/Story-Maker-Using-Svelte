<script lang='ts'>
  import { afterUpdate } from "svelte";
  import type { PageData, ActionData } from './$types';
  import { Button } from "$lib/components/ui/button/index.js";
  
  export let data: PageData;
  
  export let form: ActionData;

  let prompt = '';
  let story = '';

  let showAlert = false;
  let alertMessage = '';

  const showErrorAlert = (message: string) => {
    showAlert = true;
    alertMessage = message;
  };

  const hideAlert = () => {
    showAlert = false;
  };

  afterUpdate(() => {
    if (form?.error) {
      showErrorAlert(form.error);
    }
  });

</script>

<div class="w-100 h-[40vh] bg-gradient-to-r from-emerald-500 to-emerald-900">
  <h1  class=" relative text-[4.5rem] text-white font-bold text-center top-[3.2rem]">HotStuff</h1>
  <h1 class="text-[3rem] text-white font-bold text-center mt-7 ">Story Maker</h1>
</div>

<div class="container mx-auto p-4">


  <div class="flex flex-col gap-4">
    <form action="?/tell_story" method="POST">
      <label for="story-prompt" class="block text-gray-700 font-bold mb-2">Enter your story prompt:</label>
      <textarea
        bind:value={prompt}
        id="story-prompt"
        name="story"
        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline resize-none" 
        placeholder="Write your story prompt here..."
      ></textarea>

      <Button
        variant='destructive'
        type='submit'
        class="hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
      >
        Generate Story
      </Button>
    </form>
    <div id="story-output" class="mt-4 border rounded p-4 bg-white shadow-md">
      {form?.result}
    </div>
  </div>
</div>

{#if showAlert}
  <div class="fixed inset-0 flex items-center justify-center z-50">
    <div class="bg-white rounded-lg shadow-lg p-4">
      <p class="text-red-500">{alertMessage}</p>
      <button on:click={hideAlert} class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
        Close
      </button>
    </div>
  </div>
{/if}

<style>
  .bg-gray-100 {
    background-color: #f5f5f5;
  }

  /* Responsive Styling */
  @media (max-width: 768px) {
    .container {
      padding: 2rem; /* Adjust padding as needed */
    }

    .flex-col {
      flex-direction: column; /* Stack elements vertically */
    }

    textarea {
      height: 10rem; /* Adjust height as needed */
    }
  }
</style>
