<script>
  import { PDFViewer } from '../src/lib';
</script>

<main>
  <h1>Simple Svelte PDF Viewer</h1>
  <PDFViewer url="https://ontheline.trincoll.edu/images/bookdown/sample-local-pdf.pdf" />
</main>

<style>
  main {
    max-width: 600px;
    margin: 0 auto;
    padding: 2rem;
    font-family: 'Segoe UI', Roboto, sans-serif;
  }

  h1 {
    color: #ff3e00;
    text-align: center;
    margin-bottom: 2rem;
  }
</style>
