<script lang="ts">
	import Table from '../components/Table.svelte';
	import { gql, queryStore, createClient, dedupExchange, fetchExchange } from '@urql/svelte';

	const client = createClient({
		url: `https://swapi-graphql.netlify.app/.netlify/functions/index`,
		exchanges: [dedupExchange, fetchExchange]
	});

	$: qstore = queryStore({
		client: client,
		query: gql`
			query ExampleQuery {
				allStarships {
					edges {
						cursor
						node {
							name
						}
					}
				}
			}
		`,
		variables: {}
	});

	const qstoreEdges = () => {
		return $qstore.data.allStarships.edges;
	};
</script>

<Table {qstore} {qstoreEdges} />
