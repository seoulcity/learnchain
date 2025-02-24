# script 태그
<script lang="ts">
  import { onMount } from 'svelte';
  
  let selectedMemory: string | null = null;
  
  const memoryTypes = [
    {
      category: '베이스 메모리 🔮',
      isBase: true,
      description: '모든 메모리의 기본이 되는 추상 클래스',
      items: []
    },
    {
      category: '베이스챗 메모리 💬',
      items: [
        { 
          id: 'openai_token_buffer', 
          name: 'OpenAI 에이전트 토큰 버퍼 메모리', 
          description: 'OpenAI 모델에 최적화된 토큰 기반 버퍼 메모리' 
        },
        { 
          id: 'buffer', 
          name: '버퍼 메모리', 
          description: '대화 내용을 순차적으로 저장하는 가장 기본적인 메모리' 
        },
        { 
          id: 'conversation_summary', 
          name: '베이스 컨버세이션 서머리 메모리',
          subItems: [
            {
              id: 'summary',
              name: '컨버세이션 서머리 메모리',
              description: '전체 대화 내용을 요약하여 저장하는 스마트한 메모리'
            },
            {
              id: 'summary_buffer',
              name: '컨버세이션 서머리 버퍼 메모리',
              description: '요약본과 최근 대화를 함께 저장하는 하이브리드 메모리'
            }
          ]
        },
        { 
          id: 'buffer_window', 
          name: '버퍼 윈도우 메모리', 
          description: '최근 K개의 대화만 기억하는 효율적인 메모리' 
        },
        { 
          id: 'entity', 
          name: '엔티티 메모리', 
          description: '대화 속 중요 개체들을 추적하는 지능형 메모리' 
        },
        { 
          id: 'combined', 
          name: '컴바인드 메모리', 
          description: '여러 메모리를 조합하여 사용하는 복합 메모리' 
        },
        { 
          id: 'conversation_token_buffer', 
          name: '컨버세이션 토큰 버퍼 메모리', 
          description: '토큰 제한을 고려하여 대화를 저장하는 메모리' 
        }
      ]
    },
    {
      category: '벡터스토어 리트리버 메모리 🎯',
      items: [
        { 
          id: 'vectorstore', 
          name: '벡터스토어 메모리', 
          description: '대화 내용을 벡터화하여 저장하고 검색하는 고급 메모리' 
        }
      ]
    },
    {
      category: '제너레이티브 에이전트 메모리 🤖',
      items: [
        { 
          id: 'generative', 
          name: '제너레이티브 에이전트 메모리', 
          description: 'AI가 대화 맥락을 생성적으로 유지하는 고급 메모리' 
        }
      ]
    }
  ];

  function selectMemory(id: string) {
    selectedMemory = id;
  }
</script>

# template 태그
<div class="min-h-screen bg-gray-900 text-white p-4 md:p-8">
  <!-- Header -->
  <header class="max-w-3xl mx-auto text-center mb-8">
    <h1 class="text-3xl md:text-4xl font-bold mb-2 text-peach">백문이 불여일코딩</h1>
    <p class="text-lg text-gray-400 mb-1">100번 강의 들어도 한번 코딩 안함만 못하다 💻</p>
    <p class="text-base text-gray-500">LangChain.js 메모리 직접 체험하기 ✨</p>
  </header>

  <!-- Memory Type Selection -->
  <div class="max-w-4xl mx-auto grid gap-6">
    {#each memoryTypes as category}
      <div class="bg-gray-800/80 backdrop-blur-sm rounded-xl p-5 shadow-lg">
        <h2 class="text-xl font-semibold mb-3 text-peach flex items-center gap-2">
          {category.category}
          {#if category.isBase}
            <span class="text-xs bg-peach/20 px-2 py-1 rounded-full">Base Class</span>
          {/if}
        </h2>
        {#if category.description}
          <p class="text-sm text-gray-400 mb-3">{category.description}</p>
        {/if}
        <div class="grid grid-cols-1 gap-3">
          {#each category.items as item}
            <div class="space-y-3">
              <button
                class="w-full p-4 rounded-lg text-left transition-all duration-200 hover:scale-102
                       {selectedMemory === item.id ? 'bg-peach/20 border border-peach/30' : 'bg-gray-700/80 hover:bg-gray-600/80 border border-transparent'}"
                on:click={() => selectMemory(item.id)}
              >
                <h3 class="text-lg font-semibold mb-1">{item.name}</h3>
                {#if item.description}
                  <p class="text-sm text-gray-300">{item.description}</p>
                {/if}
              </button>
              
              {#if item.subItems}
                <div class="pl-4 space-y-2">
                  {#each item.subItems as subItem}
                    <button
                      class="w-full p-3 rounded-lg text-left transition-all duration-200 hover:scale-102
                             {selectedMemory === subItem.id ? 'bg-peach/20 border border-peach/30' : 'bg-gray-700/90 hover:bg-gray-600/90 border border-transparent'}"
                      on:click={() => selectMemory(subItem.id)}
                    >
                      <h4 class="text-base font-semibold mb-1">{subItem.name}</h4>
                      <p class="text-sm text-gray-300">{subItem.description}</p>
                    </button>
                  {/each}
                </div>
              {/if}
            </div>
          {/each}
        </div>
      </div>
    {/each}
  </div>

  <!-- Chat Interface -->
  {#if selectedMemory}
    <div class="max-w-3xl mx-auto mt-8 bg-gray-800/80 backdrop-blur-sm rounded-xl p-5 shadow-lg">
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-semibold flex items-center gap-2">
          <span class="text-peach">✨</span>
          {memoryTypes
            .flatMap(cat => cat.items)
            .flatMap(item => item.subItems ? [item, ...item.subItems] : [item])
            .find(item => item.id === selectedMemory)?.name}
        </h2>
        <button
          class="px-3 py-1.5 bg-gray-700/80 rounded-lg hover:bg-gray-600/80 transition-colors text-sm"
          on:click={() => selectedMemory = null}
        >
          다른 메모리 선택하기
        </button>
      </div>
      
      <!-- Chat Messages -->
      <div class="min-h-[400px] bg-gray-700/50 rounded-lg p-4 mb-3">
        <p class="text-center text-gray-400">채팅 인터페이스가 곧 구현될 예정입니다 🔨</p>
      </div>
      
      <!-- Input Area -->
      <div class="flex gap-2">
        <input
          type="text"
          placeholder="메시지를 입력해주세요..."
          class="flex-1 bg-gray-700/80 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-peach/50 text-sm"
        />
        <button class="px-4 py-2 bg-peach/20 text-peach border border-peach/30 rounded-lg hover:bg-peach/30 transition-colors text-sm font-medium">
          전송
        </button>
      </div>
    </div>
  {/if}
</div>

# style 태그
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

  .bg-peach\/20 {
    background-color: rgba(255, 190, 152, 0.2);
  }

  .bg-peach\/30 {
    background-color: rgba(255, 190, 152, 0.3);
  }

  .border-peach\/30 {
    border-color: rgba(255, 190, 152, 0.3);
  }

  .hover\:scale-102:hover {
    transform: scale(1.02);
  }
</style>
