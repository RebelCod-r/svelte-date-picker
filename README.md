# Svelte Date Picker

A simple date picker component built with Svelte 5 for a YouTube tutorial.

## Features

- Dark theme design
- Month navigation
- Date selection
- Built with Svelte 5 and TypeScript

## Getting Started

1. Install dependencies:

```bash
npm install
```

2. Start the development server:

```bash
npm run dev
```

3. Open your browser to `http://localhost:5173`

## Usage

```svelte
<script>
  import DatePicker from "./DatePicker.svelte";
  let selectedDate = $state(new Date());
</script>

<DatePicker bind:selectedDate />
```

## Technologies

- Svelte 5
- TypeScript
- date-fns
- Vite
