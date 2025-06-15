<script>
    import { SvelteFlowProvider } from '@xyflow/svelte';
   
    import Flow from '$lib/Flow.svelte';
    import DnDProvider from '$lib/DnDProvider.svelte';
  </script>
   
  <SvelteFlowProvider>
    <DnDProvider>
      <Flow />
    </DnDProvider>
  </SvelteFlowProvider>