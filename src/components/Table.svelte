<script lang="ts">
	import type { OperationResultStore, Pausable } from '@urql/svelte/dist/types/common';

	/**
	 * qstore is an Urql data store containing graphql query results
	 */
	export let qstore: OperationResultStore<any, {}> & Pausable;

	/**
	 * qstoreEdges is a generic function which knows how to retrieve the
	 * query-specific edges from the data store
	 */
	export let qstoreEdges = () => [];

	let dataArray: never[] = [];

	/**
	 * 
	 */
	const filterData = (_: any) => {
		let data = qstoreEdges();
		dataArray = data.filter((d: any) => {
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
			<table use:filterData>
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
