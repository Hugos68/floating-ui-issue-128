<script lang="ts">
	import { autoUpdate, flip, offset, useFloating, useHover, useInteractions, useRole } from "@skeletonlabs/floating-ui-svelte";
	import { portal } from "svelte-portal";


    interface Props {
        anime: {
			id: number;
			title: string;
			cover: string;
			ep_number: number;
		};
        sidebar_mapping: { open: boolean }[];
        idx: number;
    }

    let { anime, sidebar_mapping, idx }: Props = $props();

    const floating = useFloating({
			whileElementsMounted: autoUpdate,
			get open() {
				return sidebar_mapping[idx].open;
			},
			onOpenChange: (v) => (sidebar_mapping[idx].open = v),
			placement: 'left',
			get middleware() {
				return [
					offset(({ rects }) => {
						return rects.reference.width * 0.3;
					}),
					flip()
				];
			}
		})

		const role = useRole(floating.context, { role: 'tooltip' })
		const hover = useHover(floating.context)
		const intersections = useInteractions([role, hover])

</script>


<a
    bind:this={floating.elements.reference}
    {...intersections.getReferenceProps()}
    href="/anime/mal/{anime.id}/episode/{anime.ep_number}"
>
    <img src={anime.cover} class="w-full snap-start md:h-[5vw] md:rounded-[0.65vw]" />
</a>

{#if sidebar_mapping[idx].open}
    <div
        use:portal={'body'}
        bind:this={floating.elements.floating}
        {...intersections.getFloatingProps()}
        style="
    {floating.floatingStyles};
"
        class="!bg-[var(--dominant-color)] bg-warning text-secondary drop-shadow-[0.25vw_0.5vw_0.5vw_var(--dominant-color-opacity)] md:rounded-[0.35vw] md:px-[0.75vw] md:py-[0.25vw] md:text-[0.8vw]"
    >
        continue watching <span class="md:text-[0.9vw]">{anime.title}</span> Ep {anime.ep_number
            .toString()
            .padStart(2, '0')}
    </div>
{/if}