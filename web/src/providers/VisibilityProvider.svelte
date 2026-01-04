<script lang="ts">
  import { useNuiEvent } from '../utils/useNuiEvent';
  import { fetchNui } from '../utils/fetchNui';
  import { onMount } from 'svelte';
  import { visibility } from '../store/stores';
  import { get } from 'svelte/store';
  import type { Snippet } from 'svelte';

  let { children }: { children: Snippet } = $props();

  useNuiEvent<boolean>('setVisible', (visible) => {
    visibility.set(visible);
  });

  onMount(() => {
    const keyHandler = (e: KeyboardEvent) => {
      if (get(visibility) && ['Escape'].includes(e.code)) {
        fetchNui('hideUI');
        visibility.set(false);
      }
    };

    window.addEventListener('keydown', keyHandler);

    return () => window.removeEventListener('keydown', keyHandler);
  });
</script>

<main>
  {#if $visibility}
    {@render children()}
  {/if}
</main>
