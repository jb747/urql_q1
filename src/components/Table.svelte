<script lang="ts">
	import type { OperationResultStore, Pausable } from '@urql/svelte/dist/types/common';

	export let qstore: OperationResultStore<any, {}> & Pausable;

	let dataArray: never[] = [];

	const massageData = (node: any, data: any) => {
		dataArray = data.filter((d: any) => {
			console.log('data: ', d);
			return d.cursor == 'YXJyYXljb25uZWN0aW9uOjEz';
		});
	};
</script>

<div>
	{#if $qstore.fetching}
		<p>Loading...</p>
	{:else if $qstore.error}
		<p>Error: {$qstore.error.message}</p>
	{:else}
		<div>
			<table use:massageData={$qstore.data.allStarships.edges}>
				<thead class="header">
					<tr>
						<th>Name</th>
					</tr>
				</thead>
				<tbody>
					{#each dataArray as tableRow, i}
						<tr>
							<td>{tableRow.node.name}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	{/if}
</div>

<style>
	.scrollable {
		height: 200px;
		overflow-y: scroll;
		border-bottom: 1px solid #ddd;
	}

	.header {
		position: sticky;
		background-color: blue;
		z-index: 2;
		top: 0;
	}
</style>
