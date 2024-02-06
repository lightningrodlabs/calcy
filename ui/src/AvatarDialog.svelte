<script lang="ts">
  import "@holochain-open-dev/profiles/dist/elements/update-profile.js";

  export const close=()=>{dialog.hide()}
  export const open=()=>{dialog.show()}
  let dialog
</script>

<sl-dialog label="Edit Profile" bind:this={dialog}>
<update-profile on:cancel-edit-profile={()=>dialog.hide()} on:profile-updated={()=>dialog.hide()}></update-profile>
</sl-dialog>

<style>

  sl-dialog::part(panel) {
      background: #FFFFFF;
      border: 2px solid rgb(166 115 55 / 26%);
      border-bottom: 2px solid rgb(84 54 19 / 50%);
      border-top: 2px solid rgb(166 115 55 / 5%);
      box-shadow: 0px 15px 40px rgb(130 107 58 / 35%);
      border-radius: 10px;
  }
</style>