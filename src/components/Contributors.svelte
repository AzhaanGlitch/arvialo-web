<script>
  import { onMount } from "svelte";

  let arvialoContributors = [];
  let docsContributors = [];
  let arvialoTopContributors = [];
let docsTopContributors = [];
  let arvialoTotalContributions = 0;
  let docsTotalContributions = 0;
async function fetchContributorsData() {
    try {
      const response = await fetch('/contributors.json');
if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
}
      const data = await response.json();
// Process arvialo contributors
      if (data.arvialo) {
        arvialoTotalContributions = data.arvialo.reduce((sum, c) => sum + c.contributions, 0);
arvialoTopContributors = data.arvialo.slice(0, 3);
        const arvialoTopLogins = new Set(arvialoTopContributors.map((c) => c.login));
        arvialoContributors = data.arvialo.filter((c) => !arvialoTopLogins.has(c.login));
}
      
      // Process Docs contributors
      if (data.docs) {
        docsTotalContributions = data.docs.reduce((sum, c) => sum + c.contributions, 0);
docsTopContributors = data.docs.slice(0, 3);
        const docsTopLogins = new Set(docsTopContributors.map((c) => c.login));
        docsContributors = data.docs.filter((c) => !docsTopLogins.has(c.login));
}
      
    } catch (e) {
      console.error('Error loading contributors data:', e);
}
  }

  onMount(fetchContributorsData);
</script>

<div class="text-white max-w-6xl mx-auto py-4 px-4">
  <div class="mb-20">
      <div class="flex flex-wrap justify-center gap-6 mb-8">
        {#each arvialoTopContributors as c}
          <a
            href={c.html_url}
            target="_blank"
            class="bg-coolgray-200/70 rounded p-6 flex flex-col items-center justify-center hover:bg-coolgray-300 transition h-56 w-full sm:flex-1 sm:max-w-md text-center"
 
         >
            <img
              src={c.avatar_url}
              alt={c.login}
              class="w-20 h-20 rounded-full mb-3"
            />
            <div class="font-normal mt-1 text-lg">{c.login}</div>
       
     <div class="text-md mt-1 text-neutral-400">
              {c.contributions} contributions
            </div>
          </a>
        {/each}
      </div>
    </div>
    <div class="mt-12 text-center">
      <div class="flex justify-center gap-4 flex-col sm:flex-row">
        <a
    
      href="https://github.com/AzhaanGlitch/arvialo-web"
          target="_blank"
          class="text-base font-medium rounded p-4 px-10 text-white bg-coolgray-300 hover:bg-coolgray-400 flex items-center justify-center gap-2"
        >
          <svg
            class="icon hidden sm:block"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
    
      >
            <g
              fill="none"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
            
>
              <path d="M12 5v14m7-7H5" />
            </g>
          </svg>
          Contribute to Arvialo 
        </a>
        <a
          href="https://github.com/AzhaanGlitch/arvialo-web"
          target="_blank"
          class="text-base font-medium 
rounded p-4 px-10 text-white bg-coolgray-300 hover:bg-coolgray-400 flex items-center justify-center gap-2"
        >
          <svg
            class="icon hidden sm:block"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <g
          
    fill="none"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
            >
              <path d="M12 5v14m7-7H5" />
           
 </g>
          </svg>
          Contribute to Docs
        </a>
      </div>
    </div>
</div>