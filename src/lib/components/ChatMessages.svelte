<!-- src/lib/components/ChatMessages.svelte -->
<script lang="ts">
  import { onMount } from 'svelte';
  import type { ChatPair } from '$lib/types';

  export let messages: ChatPair[];
  
  let chatContainer: HTMLDivElement;

  onMount(() => {
    chatContainer.scrollTop = chatContainer.scrollHeight;
  });
</script>

<div 
  bind:this={chatContainer}
  class="flex-1 overflow-y-auto space-y-4 p-4"
>
  {#each messages as message}
    <div class="space-y-2">
      <div class="flex items-start gap-2">
        <div class="w-8 h-8 rounded-full bg-gray-700 flex items-center justify-center text-sm">
          👤
        </div>
        <div class="flex-1 bg-gray-700/50 rounded-lg p-3 text-sm">
          {message.human}
        </div>
      </div>
      <div class="flex items-start gap-2">
        <div class="w-8 h-8 rounded-full bg-peach/20 flex items-center justify-center text-sm">
          🤖
        </div>
        <div class="flex-1 bg-peach/10 rounded-lg p-3 text-sm">
          {message.ai}
        </div>
      </div>
    </div>
  {/each}
</div>

<style>
  div::-webkit-scrollbar {
    width: 4px;
  }

  div::-webkit-scrollbar-track {
    background: transparent;
  }

  div::-webkit-scrollbar-thumb {
    background-color: rgba(255, 190, 152, 0.2);
    border-radius: 2px;
  }
</style> 