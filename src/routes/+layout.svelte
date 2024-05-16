<script>
  import '../app.css';
  import Analytics from '$lib/Analytics.svelte';
</script>

<Analytics />

<div class="flex h-screen items-center justify-center">
  <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
    <slot />
  </div>
</div>
