<script>
    import {scale} from "svelte/transition";
let shown = false;
export function show(){
    shown = true;
}
export function hide(){
    shown = false;
}
</script>
<style>
    .modal-wrapper {
      background-color: rgb(0, 0, 0);
      background-color: rgba(0, 0, 0, 0.6);
      position: fixed;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
    }
    .modal{
        background-color: white;
        max-width: 36vw;
        padding: 1rem;
        margin: 15% auto;
        margin-left: 30%;

        height: 40%;
        border:2px solid black;
        border-radius: 10px;;
        display: block;

    }
    .close{
        float: right;
        cursor: pointer;
    }
    .close{
        font-weight: bold;
    }
</style>
{#if shown}
<div class="modal-wrapper" on:click={() => hide()}>
    <div class="modal" transition:scale={{opacity:0}}>
        <span class="close" on:click={() => hide()}>&times;</span>
     <slot />
    </div>

</div>
{/if}
