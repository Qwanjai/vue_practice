<script>
import Card from "../src/components/Card.vue";
import PokemonCards from "./components/PokemonCards.vue";

const api = "https://pokeapi.co/api/v2/pokemon";
const IDS = ["1", "4", "7"];
export default {
	components: { Card, PokemonCards },
	data() {
		return {
			pokemons: [],
			evolutions: [],
			selectedId: null,
		};
	},
	async created() {
		this.pokemons = await this.fetchData(IDS);
	},
	methods: {
		async fetchEvolutions(pokemon) {
			this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2]);
			this.selectedId = pokemon["id"];
		},
		async fetchData(ids) {
			const response = await Promise.all(ids.map((id) => window.fetch(`${api}/${id}`)));
			const jsonPokenmon = await Promise.all(response.map((data) => data.json()));
			return jsonPokenmon.map((datum) => ({
				id: datum.id,
				name: datum.name,
				sprite: datum.sprites.other["official-artwork"].front_default,
				types: datum.types.map((type) => type.type.name),
			}));
		},
	},
};
</script>

<template>
	<pokemon-cards :pokemons="pokemons" @chosen="fetchEvolutions" :selectedId="selectedId" />
	<hr />
	<pokemon-cards :pokemons="evolutions" />
</template>

<style scoped>
.cards {
	display: flex;
}
img {
	width: 100%;
}
.opace {
	opacity: 0.5;
}
.card:hover {
	opacity: 1;
}
</style>
