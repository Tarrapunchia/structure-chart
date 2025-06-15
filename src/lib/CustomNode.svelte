<script lang="ts">
  import { Handle, useConnection, Position, useSvelteFlow, useNodes, type NodeProps } from '@xyflow/svelte';
  import { X, Move } from '@lucide/svelte';
  let { id, data }: NodeProps = $props();
 
  let { updateNodeData } = useSvelteFlow();

  const connection = useConnection();
  const nodes = useNodes();
</script>

<div class="text-updater-node">
  <div
    style="position:absolute; top:-20px; right: 0px;"
  >
    <button
      style="border: none; background-color:transparent; z-index: 20; cursor:pointer;"
    >
      <Move strokeWidth={1} size={14}/>
    </button>
    <button
      style="border: none; background-color:transparent; z-index: 20; cursor:pointer;"
      onclick={() => {
        nodes.update((nds) => nds.filter((node) => node.id !== id));
      }}
    >
      <X strokeWidth={1} size={14}/>
    </button>
  </div>
  <div>
    <input
      style="
      width:100%;
      text-align: center;
      border: none;
      background-color: transparent;
      outline: none;
      resize: none;
      font-size:large;
      padding:20px 0px;
      "
      placeholder="func"
      value={data.text}
      oninput={(evt) => {
        updateNodeData(id, { text: evt.target.value });
      }}
      class="nodrag"
    >
    </div>
  {#if !connection.current.inProgress}
    <Handle
    style="
    width: 100%;
    height: 20%;
    background: blu;
    position: absolute;
    top: 0;
    left: 0;
    border-radius: 0;
    transform: none;
    border: none;
    opacity: 0;
    z-index: 1;
    "
    id="top"
    position={Position.Top}
    type="source"
    />
    <Handle
    style="
    width: 10%;
    height: 100%;
    background: blu;
    position: absolute;
    top: 0;
    right: 0;
    border-radius: 0;
    transform: none;
    border: none;
    opacity: 0;
    z-index: 1;
    "
    id="right"
    position={Position.Right}
    type="source"
    />
    <Handle
    style="
    width: 10%;
    height: 100%;
    background: blu;
    position: absolute;
    top: 0;
    right: 0;
    border-radius: 0;
    transform: none;
    border: none;
    opacity: 0;
    z-index: 1;
    "
    id="left"
    position={Position.Left}
    type="source"
    />
    <Handle
    style="
    width: 100%;
    height: 20%;
    background: blu;
    position: absolute;
    bottom: 0;
    left: 0;
    border-radius: 0;
    transform: none;
    border: none;
    opacity: 0;
    z-index: 1;
    "
    id="bottom"
    position={Position.Bottom}
    type="source"
    />
  {/if}
  <Handle
  style="
  width: 100%;
  height: 100%;
  background: transparent;
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

<style>
.text-updater-node {
  width: 150px;
  border: 1px solid black;
  border-width: thin;
  padding: 5px;
  border-radius: 5px;
  background: white;
  text-align: end;
}

</style>