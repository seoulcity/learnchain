<!-- src/routes/+page.svelte -->
<script lang="ts">
  import { onMount } from 'svelte';
  import MemoryNavigation from '$lib/components/MemoryNavigation.svelte';
  import ChatMessages from '$lib/components/ChatMessages.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import type { ChatPair } from '$lib/types';
  
  let selectedMemory: string | null = null;
  let messages: ChatPair[] = [];
  let userInput = '';
  let isNavOpen = false;
  let mainContent: HTMLElement;

  onMount(async () => {
    const response = await fetch('/src/lib/data/sample-chats.json');
    const data = await response.json();
    messages = data.pairs;
  });

  function handleMemorySelect(id: string) {
    selectedMemory = id;
    isNavOpen = false;
    // 스무스 스크롤
    if (window.innerWidth < 768) {
      mainContent?.scrollIntoView({ behavior: 'smooth' });
    }
  }

  function handleSubmit() {
    if (!userInput.trim()) return;
    
    messages = [...messages, {
      human: userInput,
      ai: "이 응답은 선택된 메모리 타입에 따라 다르게 생성될 예정입니다."
    }];
    
    userInput = '';
  }

  function toggleNav() {
    isNavOpen = !isNavOpen;
  }
</script>

<div class="min-h-screen bg-gray-900 text-white flex flex-col">
  <div class="flex-1 flex flex-col overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 w-full flex-1 flex flex-col">
      <!-- Mobile Navigation Toggle -->
      <button
        class="md:hidden fixed top-4 left-4 z-50 p-2 bg-gray-800/80 rounded-lg hover:bg-gray-700/80 transition-colors"
        on:click={toggleNav}
        aria-label="메뉴 토글"
      >
        <span class="text-2xl transform transition-transform duration-300 block
                   {isNavOpen ? 'rotate-90 scale-90' : 'rotate-0'}"
        >
          {isNavOpen ? '✕' : '☰'}
        </span>
      </button>

      <!-- Header -->
      <header class="bg-gray-800/80 backdrop-blur-sm border border-gray-700/30 rounded-xl p-4 md:p-6 mb-4 md:mb-6">
        <div class="max-w-3xl mx-auto text-center space-y-2">
          <div class="inline-block px-3 py-1 rounded-full bg-peach/10 text-peach text-xs font-medium mb-2">
            ClaS Study Project
          </div>
          <h1 class="text-xl md:text-2xl lg:text-3xl font-bold bg-gradient-to-r from-peach to-[#FFD700] bg-clip-text text-transparent">
            백문이 불여일코딩
          </h1>
          <p class="text-sm md:text-base text-gray-400">
            100번 강의 듣고 코딩 한번쯤은 해보아요 
            <span class="inline-block animate-bounce">💻</span>
          </p>
        </div>
      </header>

      <!-- Main Content Area -->
      <div class="flex-1 flex flex-col md:flex-row gap-4 md:gap-6 min-h-0">
        <!-- Navigation -->
        <div
          class="fixed md:relative md:w-72 lg:w-80 z-40 transition-all duration-300 ease-in-out
                 md:transform-none md:opacity-100
                 {isNavOpen ? 'inset-0 bg-gray-900' : '-translate-x-full opacity-0 md:opacity-100 md:translate-x-0'}"
        >
          <div class="h-full md:h-auto overflow-auto">
            <MemoryNavigation {selectedMemory} onSelect={handleMemorySelect} />
          </div>
        </div>
        
        <!-- Main Content -->
        <main 
          bind:this={mainContent}
          class="flex-1 flex flex-col min-w-0 overflow-hidden"
        >
          <!-- Chat Area -->
          <div class="flex-1 flex flex-col min-h-0">
            <div class="flex-1 bg-gray-800/80 backdrop-blur-sm rounded-xl overflow-hidden flex flex-col max-w-3xl mx-auto w-full border border-gray-700/30">
              {#if selectedMemory}
                <div class="border-b border-gray-700/30 p-4 flex justify-between items-center bg-gray-800/50">
                  <h2 class="text-base md:text-lg font-semibold flex items-center gap-2 truncate">
                    <span class="text-peach animate-pulse">✨</span>
                    {selectedMemory}
                  </h2>
                  <div class="flex items-center gap-2 shrink-0">
                    <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
                    <span class="text-xs bg-peach/10 px-2 py-1 rounded-full border border-peach/20">
                      테스트 중
                    </span>
                  </div>
                </div>
                
                <div class="flex-1 min-h-0">
                  <ChatMessages {messages} />
                </div>
                
                <div class="border-t border-gray-700/30 p-4 bg-gray-800/50">
                  <form 
                    class="flex gap-2"
                    on:submit|preventDefault={handleSubmit}
                  >
                    <input
                      type="text"
                      bind:value={userInput}
                      placeholder="메시지를 입력해주세요..."
                      class="flex-1 min-w-0 bg-gray-700/50 rounded-lg px-4 py-2 
                             focus:outline-none focus:ring-2 focus:ring-peach/30 
                             border border-gray-600/30 text-sm
                             placeholder-gray-400 transition-all duration-200"
                    />
                    <button 
                      type="submit"
                      class="px-4 py-2 bg-peach/10 text-peach border border-peach/20 
                             rounded-lg hover:bg-peach/20 transition-all duration-200 
                             text-sm font-medium whitespace-nowrap flex items-center gap-2
                             disabled:opacity-50 disabled:cursor-not-allowed shrink-0"
                      disabled={!userInput.trim()}
                    >
                      전송
                      <span class="text-xs">→</span>
                    </button>
                  </form>
                </div>
              {:else}
                <div class="flex-1 flex items-center justify-center p-4 md:p-8">
                  <div class="text-center space-y-4">
                    <div class="relative w-16 md:w-24 h-16 md:h-24 mx-auto">
                      <p class="text-4xl md:text-6xl absolute inset-0 animate-ping opacity-25">
                        {isNavOpen ? '👆' : '👈'}
                      </p>
                      <p class="text-4xl md:text-6xl relative">
                        {isNavOpen ? '👆' : '👈'}
                      </p>
                    </div>
                    <div class="space-y-2">
                      <p class="text-base md:text-lg font-medium text-gray-300">
                        메모리 타입을 선택해주세요
                      </p>
                      <p class="text-sm text-gray-400">
                        {isNavOpen ? '위' : '왼쪽'}에서 테스트하고 싶은 메모리를 선택하면
                        <br class="hidden sm:block">실시간으로 작동을 확인할 수 있어요
                      </p>
                    </div>
                  </div>
                </div>
              {/if}
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>

  <Footer />
</div>

<style>
  :global(body) {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  :global(:root) {
    --color-peach: rgb(255, 190, 152);
  }

  .text-peach {
    color: var(--color-peach);
  }

  .bg-peach\/10 {
    background-color: rgba(255, 190, 152, 0.1);
  }

  .bg-peach\/20 {
    background-color: rgba(255, 190, 152, 0.2);
  }

  .border-peach\/20 {
    border-color: rgba(255, 190, 152, 0.2);
  }

  .from-peach {
    --tw-gradient-from: rgb(255, 190, 152);
    --tw-gradient-stops: var(--tw-gradient-from), var(--tw-gradient-to);
  }

  @keyframes ping {
    75%, 100% {
      transform: scale(2);
      opacity: 0;
    }
  }

  .animate-ping {
    animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
  }
</style>
