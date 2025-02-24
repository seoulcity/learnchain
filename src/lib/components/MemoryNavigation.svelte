<!-- src/lib/components/MemoryNavigation.svelte -->
<script lang="ts">
  export let selectedMemory: string | null;
  export let onSelect: (id: string) => void;

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
</script>

<div class="w-80 bg-gray-800/80 backdrop-blur-sm h-screen overflow-y-auto p-4 border-r border-gray-700/50">
  <div class="space-y-4">
    {#each memoryTypes as category}
      <div class="space-y-2">
        <h2 class="text-sm font-semibold text-peach flex items-center gap-2">
          {category.category}
          {#if category.isBase}
            <span class="text-xs bg-peach/20 px-2 py-0.5 rounded-full">Base</span>
          {/if}
        </h2>
        {#if category.description}
          <p class="text-xs text-gray-400">{category.description}</p>
        {/if}
        <div class="space-y-1">
          {#each category.items as item}
            <button
              class="w-full text-left p-2 rounded-lg text-sm transition-all
                     {selectedMemory === item.id ? 'bg-peach/20 text-white' : 'text-gray-300 hover:bg-gray-700/50'}"
              on:click={() => onSelect(item.id)}
            >
              {item.name}
            </button>
            
            {#if item.subItems}
              <div class="pl-4 space-y-1">
                {#each item.subItems as subItem}
                  <button
                    class="w-full text-left p-2 rounded-lg text-sm transition-all
                           {selectedMemory === subItem.id ? 'bg-peach/20 text-white' : 'text-gray-300 hover:bg-gray-700/50'}"
                    on:click={() => onSelect(subItem.id)}
                  >
                    {subItem.name}
                  </button>
                {/each}
              </div>
            {/if}
          {/each}
        </div>
      </div>
    {/each}
  </div>
</div>

<style>
  /* 스크롤바 스타일링 */
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