<script lang="ts">
  import {i} from '@inlang/sdk-js';
  import {imgClicked, imgData} from './imgPreview';
  export let fileName: string;
  export let imageURL: string;
  export let resultImage: string;
  export let resultFile: string;
  const progress = 100;

  $: $imgClicked;
  $: $imgData;

  function handleImgClick() {
    $imgClicked = true;
    $imgData = resultImage;
  }
</script>

<div class="mt-6 bg-white rounded divide-y divide-gray-200 shadow">
  <div
    class="lg:grid lg:grid-rows-1 lg:grid-cols-4 lg:gap-2 md:grid md:gap-2 sm:gap-2 items-center justify-center"
  >
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div on:click={handleImgClick}>
      <img class="bg-gray-200" src={resultImage} alt="result_image" />
    </div>

    {#if fileName}
      <div
        class="lg:grid lg:grid-rows-2 lg:grid-cols-1 lg:gap-2 md:grid md:grid-cols-2 md:items-center"
      >
        <p
          class="text-base sm:text-lg md:text-xl lg:text-2xl font-sans font-bold"
        >
          {i('File Name')}
        </p>
        <p class="text-xs sm:text-sm md:text-base lg:text-lg break-words">
          {fileName}
        </p>
      </div>
    {:else if imageURL}
      <div
        class="lg:grid lg:grid-rows-2 lg:grid-cols-1 lg:gap-2 md:grid md:grid-cols-2 md:items-center"
      >
        <p
          class="text-base sm:text-lg md:text-xl lg:text-2xl font-sans font-bold"
        >
          {i('Image URL')}
        </p>

        <p class="text-xs sm:text-sm md:text-base lg:text-lg break-words">
          {imageURL}
        </p>
      </div>
    {/if}

    <div
      class="lg:grid lg:grid-rows-2 lg:grid-cols-1 md:grid md:grid-cols-2 sm:grid sm:grid-cols-2 items-center"
    >
      <p
        class="text-base sm:text-lg md:text-xl lg:text-2xl font-sans font-bold"
      >
        {i('File Status')}
      </p>
      <div
        class="bg-gray-200 rounded-full h-10 md:h-12 lg:h-14 shadow-inner overflow-hidden relative flex items-center justify-center"
      >
        <div
          class="inline-block h-full bg-yellow-300 absolute top-0 left-0"
          style="width: {progress}%"
        />
        <div
          class="relative z-10 text-xs font-semibold text-center text-white drop-shadow text-shadow"
        >
          <p class="text-sm sm:text-sm md:text-base lg:text-lg">{progress}%</p>
        </div>
      </div>
    </div>

    <div
      class="lg:grid lg:grid-rows-2 lg:grid-cols-1 md:grid md:grid-cols-2 sm:grid sm:grid-cols2 items-center"
    >
      <p
        class="text-base sm:text-lg md:text-xl lg:text-2xl font-sans font-bold"
      >
        {i('Download')}
      </p>
      <button
        class="w-full bg-cyan-300 rounded-full h-10 md:h-12 lg:h-14 shadow-inner overflow-hidden relative flex items-center justify-center"
      >
        <div
          class="relative z-10 text-xs font-semibold text-center text-white drop-shadow text-shadow"
        >
          <a
            target="_blank"
            rel="noopener"
            href={resultFile}
            download="Result_Image"
          >
            <p class="text-sm sm:text-sm md:text-base lg:text-lg">
              {i('Click')}
            </p>
          </a>
        </div>
      </button>
    </div>
  </div>
</div>
