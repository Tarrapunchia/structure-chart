<script lang="ts">
  import {
  Handle,
  Position,
  useConnection,
  type NodeProps,
} from '@xyflow/svelte';
import { Move } from '@lucide/svelte';

let { id }: NodeProps = $props();

const connection = useConnection();
</script>
<div class="rhombus">
  {#if !connection.current.inProgress}
  <Handle
  style="
  width: 100%;
  height: 100%;
  background: blue;
  position: absolute;
  top: 0;
  left: 0;
  border-radius: 0;
  transform: none;
  border: none;
  opacity: 0;
  z-index: 1;
  "
  position={Position.Right}
  type="source"
  />
  {/if}
  <Handle
  style="
  width: 100%;
  height: 100%;
  background: blue;
  position: absolute;
  top: 0;
  left: 0;
  border-radius: 0;
  transform: none;
  border: none;
  opacity: 0;
  "
  position={Position.Left}
  type="target"
  isConnectableStart={false}
  />
</div>
<div style="position: relative; top: -31px; left: 30%; height: 0px;">
  <Move size={12} color={"white"}/>
</div>

<style>
.rhombus {
  height: 30px; /* adjust to control the size  */
  aspect-ratio: 1;
  clip-path: polygon(50% 0,100% 50%,50% 100%,0 50%);
  background: #88A65E;
}

</style>