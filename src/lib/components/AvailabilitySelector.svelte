<script lang="ts">
  type Availability = 'unavailable' | 'maybe' | 'available';
  type TimeSlot = 'morning' | 'afternoon' | 'night';

  // Generate next 7 days
  const days = Array.from({ length: 7 }, (_, i) => {
    const date = new Date('2025-01-10T21:01:42-07:00');
    date.setDate(date.getDate() + i);
    return {
      date,
      formatted: date.toLocaleDateString('en-US', { 
        weekday: 'short', 
        month: 'short', 
        day: 'numeric' 
      })
    };
  });

  // Initialize availability state for each day and time slot
  let availabilityState: Record<string, Record<TimeSlot, Availability>> = {};
  days.forEach(day => {
    availabilityState[day.formatted] = {
      morning: 'unavailable',
      afternoon: 'unavailable',
      night: 'unavailable'
    };
  });

  const availabilityColors: Record<Availability, string> = {
    unavailable: 'bg-red-500 hover:bg-red-600',
    maybe: 'bg-yellow-500 hover:bg-yellow-600',
    available: 'bg-green-500 hover:bg-green-600'
  };

  const timeSlotLabels: Record<TimeSlot, string> = {
    morning: '🌅 Morning',
    afternoon: '☀️ Afternoon',
    night: '🌙 Night'
  };

  function cycleAvailability(day: string, slot: TimeSlot) {
    const states: Availability[] = ['unavailable', 'maybe', 'available'];
    const currentIndex = states.indexOf(availabilityState[day][slot]);
    const nextIndex = (currentIndex + 1) % states.length;
    availabilityState[day][slot] = states[nextIndex];
    availabilityState = { ...availabilityState }; // Trigger reactivity
  }

  export function getAvailability(): Record<string, Record<TimeSlot, Availability>> {
    return availabilityState;
  }

  function getStateEmoji(state: Availability): string {
    switch (state) {
      case 'unavailable': return '❌';
      case 'maybe': return '❓';
      case 'available': return '✅';
    }
  }
</script>

<div class="space-y-4">
  <h2 class="text-xl font-semibold mb-4">Your Availability</h2>
  
  <div class="overflow-x-auto">
    <table class="min-w-full">
      <thead>
        <tr>
          <th class="px-4 py-2 text-left">Date</th>
          {#each Object.entries(timeSlotLabels) as [slot, label]}
            <th class="px-4 py-2 text-center">{label}</th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each days as { formatted }}
          <tr class="border-t">
            <td class="px-4 py-3 font-medium">{formatted}</td>
            {#each Object.keys(timeSlotLabels) as slot}
              <td class="px-4 py-2">
                <button
                  class="w-full px-4 py-2 rounded-full text-white font-medium transition-all duration-200 
                         transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-offset-2 
                         {availabilityColors[availabilityState[formatted][slot as TimeSlot]]}"
                  on:click={() => cycleAvailability(formatted, slot as TimeSlot)}
                >
                  {getStateEmoji(availabilityState[formatted][slot as TimeSlot])}
                </button>
              </td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  <div class="mt-6 text-sm text-gray-600">
    <div class="flex items-center gap-4 justify-center">
      <div class="flex items-center gap-2">
        <span class="w-3 h-3 rounded-full bg-red-500"></span>
        <span>Not Available</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="w-3 h-3 rounded-full bg-yellow-500"></span>
        <span>Check with me</span>
      </div>
      <div class="flex items-center gap-2">
        <span class="w-3 h-3 rounded-full bg-green-500"></span>
        <span>Available</span>
      </div>
    </div>
  </div>
</div>
