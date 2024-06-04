<script lang="ts">
	import '../app.css';
	import Floating from './Floating.svelte';

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
		<Floating {sidebar_mapping} {anime} {idx} />
	{/each}
</div>
