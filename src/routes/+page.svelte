<script lang='ts'>
  import { enhance } from '$app/forms';
  import { afterUpdate } from "svelte";
  import type { PageData, ActionData, SubmitFunction } from './$types';
  import { Button } from "$lib/components/ui/button/index.js";
  




  let isLoading = false;
  
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

  const handleSubmit: SubmitFunction = () => {   
    isLoading = true; // Show the load spinner
    // ... (your existing form submission logic)
    return async ({update, result}) => {
      await update()
      isLoading = false;
    }
    // Hide the load spinner
  };

</script>

<!-- nav bar -->
<nav class="bg-gradient-to-r from-emerald-500 to-emerald-900 py-2 px-6 fixed w-full top-0 z-10"> 
  <div class="container mx-auto flex items-center justify-between">
    <a href="/" class="text-white text-2xl font-bold">HotStuff</a>
    <ul class="flex space-x-6">
      <li><a href="/" class="text-gray-300 hover:text-white">Home</a></li>
      <li><a href="/about" class="text-gray-300 hover:text-white">About</a></li>
      <li><a href="/contact" class="text-gray-300 hover:text-white">Contact</a></li>
    </ul>
  </div>
</nav>
<!-- end of nav bar -->

<!-- Banner Screen -->
<div class="w-100 h-[40vh] bg-gradient-to-r from-emerald-500 to-emerald-900">
  <h1  class=" relative text-[4.5rem] text-white font-bold text-center top-[3.2rem]">HotStuff</h1>
  <h1 class="text-[3rem] text-white font-bold text-center mt-7 ">Story Maker</h1>
</div>

<!-- end of banner screen -->

<!-- story genartor -->
<div class="container mx-auto p-4">


  <div class="flex flex-col gap-4">
    <form use:enhance={handleSubmit} action="?/tell_story" method="POST">
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
<!-- end of story generator -->

<div class="flex items-center justify-center">
  {#if isLoading}
    <div class="spinner-border animate-spin inline-block w-8 h-8 border-4 border-blue-500 rounded-full opacity-75 mr-2">🌀</div>
    <h2 class="font-bold">Your Story is Loading</h2>
  {/if}
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

<!-- <Typewriter>
  <div class="flex text-center font-bold flex-col justify-center">
    <h1  class="text-[3rem] ">WHILE YOUR HERE</h1>
    <h1  class="text-[2rem] ">WE HOPE YOU ENJOY OUR STORIES</h1>
  </div>
 </Typewriter> -->

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
