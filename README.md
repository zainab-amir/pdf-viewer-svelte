# PDF Viewer Svelte

A simple, lightweight PDF viewer component for Svelte 5 with TypeScript support. Built on top of PDF.js

![til](docs/assets/example.gif)

## Usage

### Basic Example

```svelte
<script lang="ts">
  import { PDFViewer } from 'pdf-viewer-svelte';
</script>

<PDFViewer url="path/to/your/document.pdf" />
```

## Requirements

- Svelte 5.x
- pdfjs-dist is included as a dependency

## Example

To run the example

```bash
pnpm install
pnpm run example
```

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a PR.
