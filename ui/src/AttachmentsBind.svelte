<script lang="ts">
    import "@shoelace-style/shoelace/dist/components/skeleton/skeleton.js";
    import { createEventDispatcher, getContext } from "svelte";
    import type { CalcyStore } from "./store";
    import type { Board } from "./board";
    import SvgIcon from "./SvgIcon.svelte";
    import type { AppletInfo, AttachmentType } from "@lightningrodlabs/we-applet";
    import { HoloHashMap, type EntryHashMap } from "@holochain-open-dev/utils";
    import type { EntryHash } from "@holochain/client";
    import { hashEqual } from "./util";
  
    export let activeBoard: Board
  
    const dispatch = createEventDispatcher()
    const { getStore } :any = getContext("store");
    let store: CalcyStore = getStore();
  
    type AppletTypes = {
      appletName: string,
      attachmentTypes: Record<string,AttachmentType>
    }
  
    type Groups = {
      logo_src: string,
      name: string,
    }
  
    let groups: HoloHashMap<EntryHash, Groups> = new HoloHashMap
    let appletInfos: HoloHashMap<EntryHash, AppletInfo> = new HoloHashMap
    $: attachmentTypes= []
    $: groups
  
    export const refresh = async () => {
      attachmentTypes = Array.from(store.weClient.attachmentTypes.entries())
      groups = new HoloHashMap<EntryHash, Groups>
      appletInfos = new HoloHashMap
      for (const [hash, aType] of attachmentTypes) {
          let appletInfo = appletInfos.get(hash)
          if (!appletInfo) {
              appletInfo = await store.weClient.appletInfo(hash)
              appletInfos.set(hash, appletInfo)
          } 
          for (const groupHash of appletInfo.groupsIds) {
              let groupTypes = store.weClient.attachmentTypes.get(groupHash)
              if (!groupTypes) {
                  const profile = await store.weClient.groupProfile(groupHash)
                  groups.set(groupHash, {
                      logo_src: profile.logo_src,
                      name: profile.name,
                  })
              }
          }
      }
      groups = groups
      attachmentTypes = attachmentTypes
    }  
  
  </script>
  
  <div>
      <h3>Create Bound Item From:</h3> 
      {#each Array.from(groups.entries()) as [groupHash, group]}
          <div style="display:flex;flex-direction:column">
              <div style="display:flex;align-items:center;">
                  <img width="16" style="margin-right:4px" src="{group.logo_src}"/> <strong style="font-size:105%;margin-right:4px">{group.name}:</strong>
              </div>
              <div style="margin-left: 20px;display:flex; flex-wrap:wrap">
                  {#each Array.from(store.weClient.attachmentTypes.entries()) as [appletHash, record]}
                      {@const appletInfo = appletInfos.get(appletHash)}
                      {#if appletInfo.groupsIds.find(id=>hashEqual(id,groupHash))}
                          <div style="display:flex;align-items:center;margin-right:15px">
                              <strong style="margin-right:5px;">{appletInfo.appletName}: </strong>
  
                              {#each Object.values(record) as aType,i}
                              <div style="display:flex;align-items:center;">
                                  <sl-icon style="margin-right:3px" src={aType.icon_src}></sl-icon>{aType.label}
                                  <sl-button size="small" circle style="margin-left:3px" on:click={async ()=>{
                                      const hrl = await aType.create({hrl:[store.dnaHash,activeBoard.hash]})
                                      dispatch("add-binding",hrl)
                                      }} >          
                                      <SvgIcon icon=faPlus size=10/>
                                  </sl-button>
                                  {#if i>0}<span style="margin-left:5px">,</span>{/if}
                              </div>
                              {/each}
                          </div>
                      {/if}
                  {/each}
              </div>
          
          </div>
      {/each}
  </div>
  <style>
  </style>