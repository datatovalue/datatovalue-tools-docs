<!-- Mermaid.svelte -->
<script context="module">
    import mermaid from "mermaid";
</script>

<script>
    import { onMount, createEventDispatcher } from "svelte";
    export let id;

    let config = {
        startOnLoad: true,
        theme: "neutral",
        themeVariables: {
            fontFamily: "monospace"
        }
    }

    const dispatch = createEventDispatcher();

    onMount(() => {
        if (!id) {
            throw new Error("The 'id' prop is required for the Mermaid component.");
        }

        mermaid.init(
            config,
            document.getElementById(`mermaid-container-${id}`)
        );

        // Dispatch an event to let the parent component know that the diagram is rendered
        dispatch("mermaidRendered");
    });
</script>

<div id={`mermaid-container-${id}`} class={`mermaid-container-${id} pb-4`}>
    <slot />
</div>