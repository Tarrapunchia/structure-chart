<script lang="ts">
    import {
      SvelteFlow,
      Controls,
      Background,
      BackgroundVariant,
      MarkerType,
      MiniMap,
      useSvelteFlow,
      ConnectionLineType,
      type Node,
      type Edge,
    } from '@xyflow/svelte';
    import { useDnD } from './DnDProvider.svelte';
    import Sidebar from './Sidebar.svelte';
    import CustomNode from './CustomNode.svelte';
    import WhileNode from './WhileNode.svelte';
    import CustomEdge from './CustomEdge.svelte';
    import IfNode from './IfNode.svelte';
    import VarNode from './VarNode.svelte';
   
    import '@xyflow/svelte/dist/style.css';
   
    let nodes = $state.raw<Node[]>([]);
   
    let edges = $state.raw<Edge[]>([]);
   
    const { screenToFlowPosition } = useSvelteFlow();
   
    const type = useDnD();
   
    const onDragOver = (event: DragEvent) => {
      event.preventDefault();
   
      if (event.dataTransfer) {
        event.dataTransfer.dropEffect = 'move';
      }
    };
   
    const onDrop = (event: DragEvent) => {
      event.preventDefault();
   
      if (!type.current) {
        return;
      }
   
      const position = screenToFlowPosition({
        x: event.clientX,
        y: event.clientY,
      });
   
      const newNode = {
        id: `${Math.random()}`,
        type: type.current,
        position,
        data: { label: `${type.current} node` },
        origin: [0.5, 0.0],
      } satisfies Node;
   
      nodes = [...nodes, newNode];
    };

    const nodeTypes = {
        custom: CustomNode,
        while: WhileNode,
        if: IfNode,
        var: VarNode
    };
    const edgeTypes = { 'custom-edge': CustomEdge };
    const defaultEdgeOptions = {
      type: 'custom-edge',
      markerEnd: {
        type: MarkerType.ArrowClosed,
      },
    };
  </script>
   
  <main>
    <SvelteFlow
      bind:nodes
      bind:edges
      {nodeTypes}
      {edgeTypes}
      {defaultEdgeOptions}
      fitView ondragover={onDragOver}
      connectionLineType={ConnectionLineType.Straight}
      ondrop={onDrop}
    >
      <Controls />
      <Background variant={BackgroundVariant.Dots} />
      <MiniMap />
    </SvelteFlow>
    <Sidebar />
  </main>
   
  <style>
    main {
      height: 100vh;
      display: flex;
      flex-direction: column-reverse;
    }

  </style>