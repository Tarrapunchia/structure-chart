<script lang="ts">
    import { Position, useSvelteFlow, type NodeProps } from '@xyflow/svelte';
    import { CircleX } from '@lucide/svelte';
    let { id, data }: NodeProps = $props();
   
    let { updateNodeData } = useSvelteFlow();
  </script>
   
  <div class="text-updater-node">
    <button style="text-align: center; border: none; background-color:transparent;">
      <CircleX strokeWidth={1} size={12}/>
    </button>
    <div>
      <textarea
        style="text-align: center; border-radius: 5px;"
        value={data.text}
        oninput={(evt) => {
          updateNodeData(id, { text: evt.target.value });
        }}
        class="nodrag"
      ></textarea>
    </div>
  </div>

  <style>
    .text-updater-node {
  /* height: 50px; */
  border: 1px solid black;
  padding: 5px;
  border-radius: 5px;
  background: white;
  text-align: center;
}
 
.text-updater-node label {
  display: block;
  color: #777;
  font-size: 12px;
}
  </style>