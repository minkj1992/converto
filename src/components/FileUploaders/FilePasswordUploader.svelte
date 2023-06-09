<script lang="ts">
  import type {Files} from 'filedrop-svelte';
  import {filesize} from 'filesize';
  import {IconPdf, IconX} from '@tabler/icons-svelte';
  import type {filePasswordUploadData} from './types';
  import {i} from '@inlang/sdk-js';
  import {errorStatus, errorMessage} from '@components/common/error';

  export let files: Files;
  export let uploadData: filePasswordUploadData;
  export let isDownloading: boolean;

  const formId = 'formId';

  $: $errorStatus;
  $: $errorMessage;

  function fileName(filename: string) {
    return filename.length > 10 ? filename.substring(0, 10) + '...' : filename;
  }

  function removeFile(index: number): void {
    if (index < 0 || index >= files.accepted.length) {
      return;
    }
    files.accepted = [
      ...files.accepted.slice(0, index),
      ...files.accepted.slice(index + 1)
    ];
  }

  function clearFiles(): void {
    files = {
      accepted: [],
      rejected: []
    } as Files;
  }

  async function submitFiles(e: any) {
    isDownloading = true;
    if (files.accepted.length <= 0) {
      // throw new Error("There's no files to submit");
      $errorStatus = true;
      $errorMessage = "There's no Files to Submit";
    }

    uploadData = files.accepted.map((file, i) => {
      const passwordInput = document.getElementById(
        `password_${i}`
      ) as HTMLInputElement;
      return [file, passwordInput.value];
    });
  }
</script>

<form id={formId} on:submit|once|preventDefault={submitFiles}>
  <div class="border rounded-2xl shadow-2xl shadow-slate-500 overflow-x-auto">
    <div class="flex flex-col md:px-10">
      {#each files.accepted as file, idx}
        <div class="flex flex-row items-center border-b-2 h-20 space-x-4">
          <div class="hidden sm:block">
            <IconPdf size={30} />
          </div>
          <div class="flex-auto flex flex-col sm:flex-row">
            <div class="flex-initial md:flex-auto">{fileName(file.name)}</div>

            <input
              id="password_{idx}"
              type="password"
              placeholder="type a password"
              class="input text-sm h-4 max-w-xs text-center"
              on:keydown={e => {
                if (e.key === 'Enter') e.preventDefault();
              }}
              required
            />
          </div>
          <div class="flex-initial">{filesize(file.size)}</div>
          <button
            class="btn btn-xs sm:btn-md btn-ghost"
            on:click={() => {
              removeFile(idx);
            }}
          >
            <IconX />
          </button>
        </div>
      {/each}
      <div class="mb-2 sm:mb-6" />
      <div class="join mb-2 md:relative join-horizontal">
        <button
          form={formId}
          on:click={clearFiles}
          class="flex-auto btn btn-ghost p-0 uppercase"
          >{i('Clear')}
        </button>
        <button form={formId} class="flex-auto btn btn-accent"
          >{i('Submit')}
        </button>
      </div>
    </div>
  </div>
</form>
