<script lang="ts">
    import { toPng } from 'html-to-image';
    import { useDnD } from './DnDProvider.svelte';
    import { Dot, MoveRight, ImageDown, Save, ArrowRightFromLine, Import, BrushCleaning } from '@lucide/svelte';
    import {
      getViewportForBounds,
      useNodes,
      useEdges,
      useSvelteFlow,
    } from '@xyflow/svelte';
   
    const type = useDnD();

    const nodes = useNodes();
    const edges = useEdges();
    const svelteFlow = useSvelteFlow();
    const imageWidth = 1024;
    const imageHeight = 768;
    let file_name = $state("");
    let save_name: string = $state("");
    let stored_charts = $state({});
    let option_value:string = $state("");
    let input_file = $state();
    function handleClick() {
      const nodesBounds = svelteFlow.getNodesBounds(nodes.current);
      const viewport = getViewportForBounds(
        nodesBounds,
        imageWidth,
        imageHeight,
        0.5,
        2.0,
        0.2,
      );
    
      const viewportDomNode = document.querySelector<HTMLElement>(
        '.svelte-flow__viewport',
      )!;
      
      if (viewport) {
        toPng(viewportDomNode, {
          backgroundColor: 'white',
          width: imageWidth,
          height: imageHeight,
          style: {
            width: `${imageWidth}px`,
            height: `${imageHeight}px`,
            transform: `translate(${viewport.x}px, ${viewport.y}px) scale(${viewport.zoom})`,
          },
        }).then((dataUrl) => {
          const link = document.createElement('a');
          link.download = file_name != "" ? `${file_name}.png` : 'structure-chart.png';
          link.href = dataUrl;
          link.click();
        });
      }
    }

    function download() {
      const toString = JSON.stringify({"nodes": nodes.current, "edges": edges.current});
      var element = document.createElement('a');
      element.setAttribute('href', 'data:application/json;charset=utf-8,' + encodeURIComponent(toString));
      element.setAttribute('download', file_name != "" ? `${file_name}.json` : 'structure-chart.json');

      element.style.display = 'none';
      document.body.appendChild(element);

      element.click();

      document.body.removeChild(element);
    }
    let json;
    async function getCharts() {
      let data = localStorage.getItem("charts");
      if (data) {
        const obj = await JSON.parse(data);
        stored_charts = obj; 
      } else {
        stored_charts = {};
      }
    }

    async function onChange(e) {
		  const file = e.target.files[0];
		  if (file == null) {
		  	json = null;
		  	return;
		  }
    
		  json = await readJsonFile(file);
      nodes.set(json.nodes);
      edges.set(json.edges);
	  }

    function readJsonFile(file) {
		  const reader = new FileReader();
		  return new Promise((resolve, reject) => {
		  	reader.onload = () => resolve(JSON.parse(reader.result));
		  	reader.onerror = reject;
		  	reader.readAsText(file);
		  });
	  }

    async function save_chart() {
      let charts = localStorage.getItem("charts");
      if (!charts) {
        const el = {save_name: {"nodes": nodes.current, "edges": edges.current}};
        localStorage.setItem("charts", JSON.stringify(el));
      } else {
        let obj = await JSON.parse(charts);
        obj[save_name] = {"nodes": nodes.current, "edges": edges.current}; 
        localStorage.setItem("charts", JSON.stringify(obj));
      }
    }
    const onDragStart = (event: DragEvent, nodeType: string) => {
      if (!event.dataTransfer) {
        return null;
      }
   
      type.current = nodeType;
   
      event.dataTransfer.effectAllowed = 'move';
    };
    
  </script>
   
  <aside>
    <div class="nodes-container">
      <div style="display: flex;align-items: center;">
        drag a component  to the panel
        <div
          class="input-node node"
          ondragstart={(event) => onDragStart(event, 'custom')}
          draggable={true}
        >
          func
        </div>
        <div
          style="display: flex; margin-left:10px;"
          ondragstart={(event) => onDragStart(event, 'var')}
          draggable={true}
        >
        <Dot strokeWidth={1}/>
        <div style="margin-left: -12px;;">
          <MoveRight strokeWidth={1}/>
        </div>
        </div>
        <div
          class="oval"
          style="margin-left: 20px;"
          ondragstart={(event) => onDragStart(event, 'while')}
          draggable={true}
        >
        </div>
        <div
          class="rhombus"
          style="margin-left: 20px;"
          ondragstart={(event) => onDragStart(event, 'if')}
          draggable={true}
        >
        </div>
        </div>
        <div style="display: flex; align-items: center;">
          <label for="file_name" style="font-size: large;">png</label>
          <button style="border: none; background-color:transparent; cursor: pointer;" onclick={handleClick}>
            <ImageDown strokeWidth={1}/>
          </button>
          <label for="file_name" style="font-size: large; margin-left:10px;">export</label>
          <button style="border: none; background-color:transparent; cursor: pointer;" onclick={download}>
            <ArrowRightFromLine strokeWidth={1}/>
          </button>
          <label for="file_name" style="font-size: large; margin-left:10px;">import</label>
          <input type="file" id="file_input" bind:value={input_file} style="display: none;" onchange={onChange} accept=".json"/>
          <button
            style="border: none; background-color:transparent; cursor: pointer;"
            onclick={() => document.getElementById('file_input').click()}
          >
            <Import strokeWidth={1}/>
          </button>
          <label for="save" style="font-size: large; margin-left:10px;">save</label>
          <input id="save" bind:value={save_name} style="margin-left: 10px; width: 70px;" />
          <button style="border: none; background-color:transparent; cursor: pointer;" onclick={save_chart}>
            <Save strokeWidth={1}/>
          </button>
          <label for="load" style="font-size: large; margin-left:10px;">load</label>
          <select
            bind:value={option_value}
            style="border: none;"
	        	onchange={async () => {
              let data = localStorage.getItem("charts");
              if (data) {
                const obj = await JSON.parse(data);
                nodes.set(obj[option_value].nodes);
                edges.set(obj[option_value].edges);
              }
            }}
            onclick={getCharts}
	        >
          <option value="">--choose a chart--</option>
          {#each Object.entries(stored_charts) as [key, value]}
              <option value={key}>
                {key}
              </option>
            {/each}
	        </select>
          <button style="border: none; background-color:transparent; cursor: pointer;" onclick={() => {
            nodes.set([]);
            edges.set([]);
          }}>
            <BrushCleaning strokeWidth={1}/>
          </button>
        </div>
    </div>
</aside>

   
<style>
.oval {
  height: 30px;
  width: 100px;
  overflow:hidden;
  background-color: transparent;
  border-radius: 50%;
  border-right: solid 1px;
  border-bottom: solid 1px;
  margin-left: 20px;
}

.rhombus {
  height: 25px; /* adjust to control the size  */
  aspect-ratio: 1;
  clip-path: polygon(50% 0,100% 50%,50% 100%,0 50%);
  background: #88A65E;
  margin-left: 20px;
}

aside {
  width: 100%;
  background: #fff;
  font-size: 12px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.nodes-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.node {
  margin: 0.5rem;
  border: 1px solid #111;
  padding: 0.5rem 1rem;
  font-weight: 700;
  border-radius: 5px;
  cursor: grab;
}
</style>