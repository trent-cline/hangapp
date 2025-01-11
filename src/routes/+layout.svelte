<script lang="ts">
  import '../app.css';
  import Sidebar from '$lib/components/Sidebar.svelte';
</script>

<div class="min-h-screen bg-gray-100">
  <Sidebar />
  <main class="ml-64 p-8">
    <slot />
  </main>
</div>
