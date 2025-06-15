<script lang="ts">
    import { useSvelteFlow, type NodeProps } from '@xyflow/svelte';
    
    let { updateNodeData } = useSvelteFlow();

  </script>
  <div class="rhombus">
  </div>

  <style>
.rhombus {
  height: 20px; /* adjust to control the size  */
  aspect-ratio: 1;
  clip-path: polygon(50% 0,100% 50%,50% 100%,0 50%);
  background: #88A65E;
}
  </style>