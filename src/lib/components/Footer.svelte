<!-- src/lib/components/Footer.svelte -->
<script lang="ts">
  const teams = [
    { name: 'AIOps', message: '인공지능으로 더 나은 운영을 만드는 혁신가들', emoji: '🤖' },
    { name: 'DevOps', message: '개발과 운영의 경계를 허무는 선구자들', emoji: '🚀' },
    { name: 'UI/UX', message: '사용자 경험을 디자인하는 창조가들', emoji: '🎨' },
    { name: 'DX사업실', message: '디지털 혁신을 선도하는 비즈니스 리더들', emoji: '💡' }
  ];

  const currentYear = new Date().getFullYear();
</script>

<footer class="bg-gray-800/80 backdrop-blur-sm border-t border-gray-700/30 py-12 px-4">
  <div class="max-w-7xl mx-auto space-y-12">
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
      {#each teams as team}
        <div class="space-y-3 group">
          <div class="flex items-center gap-2">
            <span class="text-2xl group-hover:scale-125 transition-transform duration-300">
              {team.emoji}
            </span>
            <h3 class="text-peach font-semibold">{team.name}</h3>
          </div>
          <p class="text-sm text-gray-400 leading-relaxed">{team.message}</p>
        </div>
      {/each}
    </div>

    <div class="text-center space-y-6">
      <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-peach/5 border border-peach/10">
        <span class="text-peach">ClaS</span>
        <span class="text-xs text-gray-400">Study Project</span>
      </div>

      <p class="text-sm text-gray-400 max-w-xl mx-auto leading-relaxed">
        "배움에는 끝이 없고, 성장에는 한계가 없습니다.
        <br class="hidden sm:block" />
        여러분의 열정이 클라비의 미래를 만듭니다."
      </p>
      
      <div class="space-y-4">
        <div class="flex items-center justify-center gap-3 text-xs text-gray-500">
          <a 
            href="https://creativecommons.org/licenses/by-nc-sa/4.0/" 
            target="_blank" 
            rel="noopener noreferrer"
            class="hover:text-peach transition-colors flex items-center gap-1"
          >
            <span class="text-base">©</span>
            Creative Commons BY-NC-SA 4.0
          </a>
          <span class="w-1 h-1 rounded-full bg-gray-700"></span>
          <a 
            href="https://github.com/seoulcity" 
            target="_blank" 
            rel="noopener noreferrer"
            class="hover:text-peach transition-colors flex items-center gap-1"
          >
            <span class="i-carbon-logo-github text-base"></span>
            GitHub
          </a>
        </div>

        <p class="text-xs text-gray-500 flex items-center justify-center gap-2">
          <span>Created with</span>
          <span class="text-red-400 animate-pulse">♥</span>
          <span>by Kim JungHyun (jhkim) at ClaBi {currentYear}</span>
        </p>
      </div>
    </div>
  </div>
</footer> 