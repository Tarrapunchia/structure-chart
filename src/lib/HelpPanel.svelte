<script>
    console.log("help panel loaded");
  export let onClose;
</script>

<div
  style="
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  "
  on:click={() => onClose()}
>
  <div id="help-panel"
    on:click|stopPropagation
    style="
      background: white;
      padding: 2rem;
      border-radius: 12px;
      max-width: 600px;
      width: 90%;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
      position: relative;
    ">
    <h2>Help</h2>
    <ul style="margin-top: 1rem;">
      <li>Create a function node</li>
      <li>Link nodes together</li>
      <li>Create a while loop (the oval in the top right. Resize it to include all desired nodes)</li>
      <li>If statement (the green rhombus)</li>
      <li>
        Declare a variable. Click the arrow to switch the dot between empty and full.
        Click under the control buttons to insert the variable name
      </li>
      <li>
        You can remove any node or edge.
        For edges, click on one and press 'Delete' or 'Backspace'
      </li>
    </ul>
    <button on:click={onClose}
      style="
        margin-top: 1.5rem;
        background: #eee;
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 8px;
        cursor: pointer;
      ">
      Close Panel
    </button>
  </div>
</div>
