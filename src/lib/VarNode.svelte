<script lang="ts">
    import { NodeResizer, useNodes, useSvelteFlow, type NodeProps } from '@xyflow/svelte';
    import { Dot, MoveRight, Move, X, Lock, LockOpen, RotateCcw } from '@lucide/svelte';

    let { data, selected }: NodeProps = $props();
    let { updateNodeData } = useSvelteFlow();

    const nodes = useNodes();

    let rotation = $state(0);
    let isDragging = $state(false);
    let stroke = $state(0.5);
    let startX = $state(0);
    let startRotation = $state(0);
    let rotatableDivElement;

    function handleMouseDown(e: MouseEvent) {
        isDragging = true;
        startX = e.clientX;
        startRotation = rotation;
        if (rotatableDivElement) {
            rotatableDivElement.style.cursor = 'grabbing';
        }
    }


    function handleMouseMove(e: MouseEvent) {
        if (!isDragging) return;

        const deltaX = e.clientX - startX;
        const sensitivity = 0.5;
        rotation = startRotation + (deltaX * sensitivity);
    }

    function handleMouseUp() {
        isDragging = false;
        if (rotatableDivElement) {
            rotatableDivElement.style.cursor = 'grab';
        }
    }

    $effect(() => {
        document.addEventListener('mousemove', handleMouseMove);
        document.addEventListener('mouseup', handleMouseUp);

        return () => {
            document.removeEventListener('mousemove', handleMouseMove);
            document.removeEventListener('mouseup', handleMouseUp);
        };
    });

  </script>
  
<div >

    <div
      style="display: flex; flex-direction: column;"
      style:transform={`rotate(${rotation}deg)`}
    >
    <div style="display: flex; flex-direction: column; text-align:center" style:transform={`rotate(${-rotation}deg)`}>
        <div>
        <button style="border: none; background-color: transparent; cursor:pointer">
            <Move strokeWidth={1} size={8}/>
        </button>
        <button style="border: none; background-color: transparent;"
          bind:this={rotatableDivElement}
          onmousedown={handleMouseDown}
          class="nodrag"
          style:cursor="pointer"
        >
            <RotateCcw strokeWidth={1} size={8}/>
        </button>
        <button style="border: none; background-color: transparent;"
          style:cursor="pointer"
          onclick={() => {
            nodes.update((nds) => nds.filter((node) => node.id !== id));
          }}
        >
            <X strokeWidth={1} size={8}/>
        </button>
        </div>
        <input
        style="
        border: none;
        background-color: transparent;
        border-width: thin;
        outline: none;
        width:70px;
        margin-bottm: -10px;
        font-weight:lighter;
        text-align:center;
        "
        />
    </div>
        <div
          style="display: flex;"
          
        >
            <div>
                <div
                class="nodrag"
                >
                <button style="border: none; background-color: transparent; cursor:pointer; display:flex;" onclick={() => {
                    if (stroke == 0.5) stroke = 2;
                    else stroke = 0.5;
                }}>
                    <Dot strokeWidth={stroke} size={38}/>
                    <div style="margin-left: -19px;">
                        <MoveRight strokeWidth={0.5} size={38}/>  
                    </div>
                </button>
            </div>
        </div>
    </div>
</div>
</div>
<style>

</style>