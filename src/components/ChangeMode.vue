<template>
	<span id="changeMode">
		<p class="font-weight-medium" v-if="mode == 'get'" @click="$emit('addMode')">Adicionar Frases →</p>
		<p class="font-weight-medium" v-else @click="$emit('getMode')">← Pegar Frases</p>
	</span>
</template>

<script>
export default {
	name: 'ChangeMode',
	props: {
		mode: {
			type: String,
			required: true,
		},
	},
}
</script>

<style lang="scss">
#changeMode {
	p {
		display: inline;
		border-bottom: 1px solid white;
		text-transform: lowercase;
		cursor: pointer;

		position: relative;
		top: -16px;

		color: white;

		&:hover {
			color: rgb(215, 215, 215);
			border-bottom: 1px solid rgb(215, 215, 215);
		}
	}
}
</style>
