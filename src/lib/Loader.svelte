<script>
    import { Circle } from 'svelte-loading-spinners';
</script>

<div class="loader">
    <Circle color="#f44336" />
</div>

<style>
    .loader {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: rgb(255 255 255 / 50%);
    }
</style>
