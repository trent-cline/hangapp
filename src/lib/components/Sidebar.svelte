<script lang="ts">
  import { page } from '$app/stores';
</script>

<nav class="h-screen w-64 bg-gray-800 text-white fixed left-0 top-0 p-4">
  <div class="mb-8">
    <h1 class="text-2xl font-bold">Hang</h1>
  </div>
  
  <ul class="space-y-2">
    <li>
      <a 
        href="/" 
        class="block py-2 px-4 rounded hover:bg-gray-700 {$page.url.pathname === '/' ? 'bg-gray-700' : ''}"
      >
        Home
      </a>
    </li>
    <li>
      <a 
        href="/request-hangout" 
        class="block py-2 px-4 rounded hover:bg-gray-700 {$page.url.pathname === '/request-hangout' ? 'bg-gray-700' : ''}"
      >
        Request Hangout
      </a>
    </li>
    <li>
      <a 
        href="/friends" 
        class="block py-2 px-4 rounded hover:bg-gray-700 {$page.url.pathname === '/friends' ? 'bg-gray-700' : ''}"
      >
        Friends
      </a>
    </li>
  </ul>
</nav>
