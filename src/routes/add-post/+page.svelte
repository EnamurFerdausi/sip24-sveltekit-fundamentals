<script>
   let  files =null;
   let imgurl='';
   let selectedFilter = '';
   

   function handleFileChange(event) {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      imgurl = e.target.result;
    };
    reader.readAsDataURL(file);
  }
}

// // Function to apply selected filter
// function applyFilter(filterName) {
//     selectedFilter = filterName;
//   }  // Function to reset filters (set to normal)
//   function resetFilters() {
//     selectedFilter = ''; // Reset selected filter
//   }
function applyFilter(filterName) {
    selectedFilter = filterName;

    if (imgurl) {
        const image = new Image();
        image.onload = function () {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            canvas.width = image.width;
            canvas.height = image.height;

            // Apply filter
            ctx.filter = selectedFilter;
            ctx.drawImage(image, 0, 0, canvas.width, canvas.height);

            // Update imgurl with filtered image data
            imgurl = canvas.toDataURL();

            // Trigger Svelte reactivity to update the image preview
            $: console.log(imgurl); // This line triggers reactivity in Svelte
        };
        image.src = imgurl;
    }
}

 // Function to reset filters (set to normal)
 function resetFilters() {
        selectedFilter = ''; // Reset selected filter
    }
  
</script>

<header class="bg-white py-4 shadow-md sticky top-0 z-10">
    <div class="container mx-auto px-4 flex justify-between items-center">
        <h1 class="text-2xl font-bold">Craftlab</h1>
        <a href="/" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Back</a>
    </div>
</header>

<form class="container mx-auto p-5" method="POST" enctype="multipart/form-data">
    <label for="dropzone" class="mb-3 flex-col items-center justify-center w-full h-64 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer">
        <div class="flex flex-col-1 items-center justify-center pt-5 pb-6">
            {#if imgurl}
                <img src="{imgurl}" class="w-[300px] image-preview" style="--filter: {selectedFilter}">
            {:else}
                <svg class="w-8 h-8 mb-4 text-gray-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16V8a4 4 0 0110.38-2.412M7 16h10M7 16v-2a4 4 0 00-4-4m14 6v-2a4 4 0 00-4-4m-4 4v4m0 0H4m16 0h-6"></path></svg>
                <p class="text-sm text-gray-500 font-semibold">Click to Upload</p>
            {/if}
        </div>
        <input bind:files={files} on:change={handleFileChange}  name="image" id="dropzone" type="file" accept="image/*" class="hidden" required />
    </label>
    <div class="flex justify-between mt-4">        <button  type="button" on:click={() => applyFilter('grayscale(100%)')} class="bg-blue-400 py-2 px-3 rounded-lg text-white filter-button">Grayscale</button>
        <button  type="button" on:click={() => applyFilter('sepia(100%)')} class="bg-yellow-400 py-2 px-3 rounded-lg text-white filter-button">Sepia</button>
        <button  type="button" on:click={() => applyFilter('brightness(150%)')} class="bg-purple-400 py-2 px-3 rounded-lg text-white filter-button">Brightness</button>        <button  type="button" on:click={resetFilters} class="bg-red-600 py-2 px-3 rounded-lg text-white">Reset</button>
      </div>

    <div class="mb-3">
        <label for="username" class="block mb-2 text-sm font-medium text-gray-900">Username</label>
        <input name="username" id="username" type="text" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg w-full p-2.5" required>
    </div>

    <div class="mb-3">
        <label for="content" class="block mb-2 text-sm font-medium text-gray-900">Content</label>
        <textarea name="content" id="content" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg w-full p-2.5" required></textarea>
    </div>

   

    <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5">Submit</button>
</form>
<style>
    .image-preview {
       filter:var(--filter)
    }
</style>