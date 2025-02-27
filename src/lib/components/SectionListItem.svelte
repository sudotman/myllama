<script lang="ts">
	import LL from '$i18n/i18n-svelte';
	import { page } from '$app/stores';
	import { Sitemap } from '$lib/sitemap';

	import ButtonDelete from './ButtonDelete.svelte';
	import Metadata from './Metadata.svelte';

	interface Props {
		sitemap: Sitemap;
		id: string;
		title: string;
		subtitle: string;
	}

	let { sitemap, id, title, subtitle }: Props = $props();

	let shouldConfirmDeletion = $state(false);

	const isSession = sitemap === Sitemap.SESSIONS;
</script>

<!-- Need to use `#key id` to re-render the delete nav after deletion -->
{#key id}
	<div
		class="section-list-item"
		class:section-list-item--active={$page.url.pathname.includes(id)}
		class:section-list-item--confirm-deletion={shouldConfirmDeletion}
	>
		<a
			class="section-list-item__a"
			data-testid={isSession ? 'session-item' : 'knowledge-item'}
			aria-label={(isSession ? $LL.session() : $LL.knowledge()) + `: ${id}`}
			href={`/${sitemap}/${id}`}
		>
			<p class="section-list-item__title">
				{title}
			</p>
			<Metadata>
				{subtitle}
			</Metadata>
		</a>
		<nav
			class="section-list-item__delete"
			class:section-list-item__delete--confirm-deletion={shouldConfirmDeletion}
		>
			<ButtonDelete {sitemap} {id} bind:shouldConfirmDeletion />
		</nav>
	</div>
{/key}

<style lang="postcss">
	.section-list-item {
		@apply flex flex-row items-center justify-between border-b pr-3;
		@apply last:border-b-0;

		&--confirm-deletion {
			@apply confirm-deletion;
		}
	}

	.section-list-item:hover .section-list-item__delete {
		@apply opacity-100;
	}

	.section-list-item__delete {
		@apply opacity-0;
	}

	.section-list-item__delete--confirm-deletion {
		@apply opacity-100;
	}

	.section-list-item--active {
		@apply bg-shade-1;
	}

	.section-list-item__a {
		@apply relative z-0 w-full overflow-hidden text-ellipsis px-6 py-3;
		@apply hover:text-active;
	}

	.section-list-item__title {
		@apply max-w-full truncate whitespace-nowrap text-sm font-bold;
	}
</style>
