<script lang="ts">
	import LL from '$i18n/i18n-svelte';
	import Button from '$lib/components/Button.svelte';
	import Fieldset from '$lib/components/Fieldset.svelte';
	import P from '$lib/components/P.svelte';
	import { StorageKey } from '$lib/localStorage';

	function deleteStorage(item: StorageKey): void {
		const dialogText: Partial<Record<StorageKey, string>> = {
			[StorageKey.myllamaSessions]: $LL.areYouSureYouWantToDeleteAllSessions(),
			[StorageKey.myllamaKnowledge]: $LL.areYouSureYouWantToDeleteAllKnowledge(),
			[StorageKey.myllamaSettings]: $LL.areYouSureYouWantToDeleteAllSettings()
		};
		if (confirm(dialogText[item])) {
			localStorage.removeItem(item);
			if (item === StorageKey.myllamaSettings) localStorage.removeItem(StorageKey.myllamaServers);
			location.reload();
		}
	}
</script>

<Fieldset>
	<P><strong>{$LL.dangerZone()}</strong></P>
	<Button variant="outline" on:click={() => deleteStorage(StorageKey.myllamaSessions)}>
		{$LL.deleteAllSessions()}
	</Button>
	<Button variant="outline" on:click={() => deleteStorage(StorageKey.myllamaKnowledge)}>
		{$LL.deleteAllKnowledge()}
	</Button>
	<Button variant="outline" on:click={() => deleteStorage(StorageKey.myllamaSettings)}>
		{$LL.deleteAllSettings()}
	</Button>
</Fieldset>
