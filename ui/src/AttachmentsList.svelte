<script lang="ts">
  import "@shoelace-style/shoelace/dist/components/skeleton/skeleton.js";
  import { createEventDispatcher, getContext } from "svelte";
  import type { CalcyStore } from "./store";
  import { hrlB64WithContextToRaw } from "./util";
  import type { HrlB64WithContext } from "@lightningrodlabs/we-applet";
  import SvgIcon from "./SvgIcon.svelte";
  import { hrlToString } from "@holochain-open-dev/utils";

  const dispatch = createEventDispatcher()

  export let attachments: Array<HrlB64WithContext>
  export let allowDelete = true

  const { getStore } :any = getContext("store");
  let store: CalcyStore = getStore();
  
</script>
<div class="attachments-list">
  {#each attachments as attachment, index}
    {@const hrlWithContext = hrlB64WithContextToRaw(attachment)}
    <div 
      class:attachment-item-with-delete={allowDelete}
      class:attachment-item={!allowDelete}
    >
      {#await store.weClient.attachableInfo(hrlWithContext)}
        <div style="cursor:pointer; padding: 0 5px 0 5px; border: dashed 1px;margin-right:5px" title={`${hrlToString(hrlWithContext.hrl)}?${JSON.stringify(hrlWithContext.context)}`}> ?...</div>
      {:then { attachableInfo }}
        <sl-button  size="small"
          on:click={ async (e)=>{
              e.stopPropagation()
              try {
                await store.weClient.openHrl(hrlWithContext)
              } catch(e) {
                alert(`Error opening link: ${e}`)
              }
            }}
          style="display:flex;flex-direction:row;margin-right:5px"><sl-icon src={attachableInfo.icon_src} slot="prefix"></sl-icon>
          {attachableInfo.name}
        </sl-button> 
      {:catch error}
        Oops. something's wrong.
      {/await}
      {#if allowDelete}
        <sl-button size="small"
          on:click={()=>{
            dispatch("remove-attachment",index)
          }}
        >
          <SvgIcon icon=faTrash size=12 />
        </sl-button>
      {/if}
</div>
  {/each}
</div>
<style>
  .attachments-list {
    display:flex;
    flex-direction:row;
    flex-wrap: wrap;
  }
  .attachment-item {
  }
  .attachment-item-with-delete {
    border:1px solid #aaa; 
    background-color:rgba(0,255,0,.1); 
    padding:4px;
    display:flex;
    margin-right:4px;
    border-radius:4px;
  }
</style>