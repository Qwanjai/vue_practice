<template>
	<div class="cards">
		<card v-for="pokemon in pokemons" :key="pokemon.id" @click="click(pokemon)" :class="{ opace: selectedId && pokemon.id !== selectedId }" class="card">
			<template v-slot:title>{{ pokemon.name }} #{{ pokemon.id }}</template>

			<template v-slot:content><img :src="pokemon.sprite" /></template>

			<template v-slot:description>
				<div v-for="t in pokemon.types" :key="t">
					{{ t }}
				</div>
			</template>
		</card>
	</div>
</template>

<script>
import Card from "./Card.vue";

export default {
	components: { Card },
	emits: ["chosen"],
	props: {
		pokemons: {
			type: Array,
			default: [],
		},
		selectedId: {
			type: Number,
		},
	},
	methods: {
		click(pokemon) {
			this.$emit("chosen", pokemon);
		},
	},
};
</script>

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
