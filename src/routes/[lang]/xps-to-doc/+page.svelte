<script lang="ts">
  import DragDrop from '@components/common/DragDrop.svelte';
  import {onDestroy} from 'svelte';
  import type {FileDropOptions, Files} from 'filedrop-svelte';
  import Downloader from '@components/common/Downloader.svelte';

  import {fetchXpsToDoc} from '$lib/apis'; // TODO: change
  import BaseUploader from '@components/FileUploaders/FileUploader.svelte';
  import type {fileUploadData} from '@components/FileUploaders/types';
  import Feature from '@components/home/Feature.svelte';

  let fileDropOptions: FileDropOptions = {
    accept: ['.xps'], // TODO: change
    hideInput: true,
    multiple: true
  };
  let files: Files = {
    accepted: [],
    rejected: []
  };

  let uploadData: fileUploadData = [];
  let isDownloading = false;
  $: isFileExist = files.accepted.length > 0;

  onDestroy(() => {
    files = {
      accepted: [],
      rejected: []
    };
  });
</script>

<div class="w-full px-2 py-32 sm:px-20 lg:px-32 text-center mx-auto">
  <div class="join">
    <h2 class="text-[clamp(2rem,6vw,4.5rem)] font-black join-item">
      XPS to&nbsp<span class="text-[#37CDBE]">DOC</span>
    </h2>
  </div>
  <p class="text-base-content/60 font-title py-4 font-light md:text-2xl">
    Convert XPS to DOC online for free
  </p>

  <section class="pt-4 mx-0 sm:mx-10">
    {#if isDownloading}
      <Downloader fetchFn={fetchXpsToDoc} {uploadData} />
    {:else if isFileExist}
      <BaseUploader bind:files bind:uploadData bind:isDownloading />
    {:else}
      <DragDrop bind:files {fileDropOptions} />
    {/if}
  </section>

  <div class="mt-10" />
  <Feature />
</div>
