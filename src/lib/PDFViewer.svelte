<script lang="ts">
  import type { PDFDocumentProxy } from 'pdfjs-dist';
  import * as pdfjs from 'pdfjs-dist';
  import { onMount } from 'svelte';

  const { url }: { url: string } = $props();

  let containerElement = $state<HTMLDivElement>();
  let pdfDocument = $state<PDFDocumentProxy>();
  let totalPages = $state(0);
  let isLoading = $state(true);
  let hasError = $state(false);

  onMount(async () => {
    pdfjs.GlobalWorkerOptions.workerSrc = `https://cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjs.version}/pdf.worker.min.mjs`;

    try {
      pdfDocument = await pdfjs.getDocument(url).promise;
      totalPages = pdfDocument.numPages;
      isLoading = false;
    } catch (error) {
      console.error(error);
      hasError = true;
      isLoading = false;
    }
  });

  // Render all pages when PDF is loaded.
  $effect(() => {
    if (pdfDocument && containerElement && !isLoading) {
      void renderAllPages();
    }
  });

  async function renderAllPages() {
    if (!pdfDocument || !containerElement) return;

    containerElement.innerHTML = '';

    for (let pageNum = 1; pageNum <= totalPages; pageNum++) {
      try {
        const page = await pdfDocument.getPage(pageNum);
        const viewport = page.getViewport({scale: 1.5});

        const canvas = document.createElement('canvas');
        const context = canvas.getContext('2d');

        if (!context) {
          continue;
        }

        canvas.height = viewport.height;
        canvas.width = viewport.width;
        canvas.className = 'pdf-page';

        containerElement.appendChild(canvas);
        const renderContext = {
          canvasContext: context,
          viewport,
        };

        await page.render(renderContext).promise;
      } catch (error) {
        console.error(error);
      }
    }
  }
</script>

{#if isLoading}
  <div>Loading PDF...</div>
{:else if hasError}
  <div>Failed to load pdf</div>
{:else}
  <div class="pdf-container" bind:this={containerElement}></div>
{/if}

<style>
  .pdf-container {
    max-height: 100vh;
    overflow-y: auto;
    overflow-x: auto;
  }

  :global(.pdf-page) {
    max-width: 100%;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
</style>
