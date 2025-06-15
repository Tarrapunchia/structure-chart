<script lang="ts">
    import { NodeResizer, useSvelteFlow, type NodeProps } from '@xyflow/svelte';
    import { CircleX } from '@lucide/svelte';
    
    let { data, selected }: NodeProps = $props();
    let { updateNodeData } = useSvelteFlow();

  </script>
  
  <NodeResizer
  minWidth={100}
  minHeight={30}
  isVisible={selected}
  color="rgb(255, 64, 0)"
/>
  <div class="oval">
  </div>

  <style>
.oval {
  min-height: 30px;
  min-width: 100px;
  height: 100%;
  width: 100%;
  overflow:hidden;
  background-color: transparent;
  border-radius: 50%;
  border-right: solid 1px;
  border-bottom: solid 1px;
}
  </style>