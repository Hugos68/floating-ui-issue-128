<script lang="ts">
	import '../app.css';
	import {
		autoUpdate,
		flip,
		offset,
		useFloating,
		useHover,
		useInteractions,
		useRole
	} from '@skeletonlabs/floating-ui-svelte';
	import { portal } from 'svelte-portal';

	const sidebar_animes = [
		{
			id: 1,
			title: 'Jujutsu Kaisen season 2',
			cover: '/images/mock/cover/jjk.webp',
			ep_number: 2
		},
		{
			id: 2,
			title: 'One Piece',
			cover: '/images/mock/cover/one_piece.webp',
			ep_number: 20
		},
		{
			id: 3,
			title: 'Kaiju no.8',
			cover: '/images/mock/cover/kaiju_no_8.jpg',
			ep_number: 9
		},
		{
			id: 4,
			title: 'Demon Slayer Hashira Training Arc',
			cover: '/images/mock/cover/demon_slayer_training.webp',
			ep_number: 10
		}
	];
	const sidebar_mapping = $state(
		sidebar_animes.map(() => ({
			open: false,
			color: undefined
		}))
	);
</script>

<div class="flex flex-row">
	{#each sidebar_animes as anime, idx}
		{@const floating = useFloating({
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
		})}

		{@const role = useRole(floating.context, { role: 'tooltip' })}
		{@const hover = useHover(floating.context)}
		{@const intersections = useInteractions([role, hover])}

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
				transition:blur={{ duration: 250 }}
				class="!bg-[var(--dominant-color)] bg-warning text-secondary drop-shadow-[0.25vw_0.5vw_0.5vw_var(--dominant-color-opacity)] md:rounded-[0.35vw] md:px-[0.75vw] md:py-[0.25vw] md:text-[0.8vw]"
			>
				continue watching <span class="md:text-[0.9vw]">{anime.title}</span> Ep {anime.ep_number
					.toString()
					.padStart(2, '0')}
			</div>
		{/if}
	{/each}
</div>
