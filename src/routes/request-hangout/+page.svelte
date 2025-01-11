<script lang="ts">
  import AvailabilitySelector from '$lib/components/AvailabilitySelector.svelte';
  let availabilitySelector: AvailabilitySelector;
  let loading = false;
  
  async function requestHangout() {
    loading = true;
    try {
      const availability = availabilitySelector.getAvailability();
      console.log('Availability preferences:', availability);
      // TODO: Implement API call to check calendars and find available time
      await new Promise(resolve => setTimeout(resolve, 1000)); // Simulated API call
      alert('Found next available time: Tomorrow at 6 PM'); // Placeholder response
    } catch (error) {
      console.error('Error finding available time:', error);
      alert('Failed to find available time. Please try again.');
    } finally {
      loading = false;
    }
  }
</script>

<div class="max-w-2xl mx-auto">
  <h1 class="text-3xl font-bold mb-8">Request a Hangout</h1>
  
  <div class="bg-white rounded-lg shadow-md p-6 mb-6">
    <AvailabilitySelector bind:this={availabilitySelector} />
  </div>
  
  <div class="bg-white rounded-lg shadow-md p-6">
    <p class="mb-6 text-gray-600">
      Click below to find the next available time when all your friends can hang out.
      We'll check everyone's calendar and your preferences above.
    </p>
    
    <button
      on:click={requestHangout}
      class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 px-6 rounded-lg
             transition duration-200 ease-in-out transform hover:scale-105 
             disabled:opacity-50 disabled:cursor-not-allowed"
      disabled={loading}
    >
      {#if loading}
        <span class="inline-block animate-spin mr-2">↻</span>
        Finding time...
      {:else}
        Find Next Available Time
      {/if}
    </button>
  </div>
</div>
