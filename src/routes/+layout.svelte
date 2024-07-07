<script lang="ts">
    import type { LayoutData } from './$types';
    import '../app.css'
    export let data: LayoutData;
</script>

<main class="flex justify-center  h-full items-center ">
    <slot></slot>
</main>