<template>
	<div id="GetPhrase">
		<h1 class="white--text mt-10 mb-14">{{ catchPhrase }}</h1>
		<v-btn elevation="2" color="#537EC5" @click="getRandomPhrase" class="font-weight-bold white--text mb-12 getButton"
			><v-icon left dark>mdi-autorenew</v-icon>Pegar frase</v-btn
		>
		<div class="groupChips">
			<v-chip
				v-for="(group, index) in groups"
				:key="group.name"
				@click="selectGroup(index)"
				class="text-subtitle-1 mx-2 mb-2 pr-0"
				:color="groupSelected.index === index ? '#F39422' : '#537EC5'"
				label
				:outlined="groupSelected.index !== index"
			>
				<span class="font-weight-black white--text mr-1">{{ group.name }}</span>

				<v-menu>
					<template v-slot:activator="{ on, attrs }">
						<v-btn dark right icon v-bind="attrs" v-on="on">
							<v-icon>mdi-dots-vertical</v-icon>
						</v-btn>
					</template>
					<v-list>
						<v-list-item link @click="$emit('editMode', index)">
							<v-list-item-title>Editar</v-list-item-title>
						</v-list-item>
						<v-list-item link @click="$emit('deleteGroup', index)">
							<v-list-item-title>Apagar</v-list-item-title>
						</v-list-item>
					</v-list>
				</v-menu>
			</v-chip>
		</div>
	</div>
</template>

<script>
// ==================== VERIFICAR SE QUANDO APAGAR O GRUPO O GRUPO SELECTED É ALTERADO ====================
export default {
	name: 'GroupList',
	props: {
		groups: {
			type: Array,
			required: true,
		},
	},
	data() {
		return {
			groupSelected: {
				index: 0,
				phrases: [],
			},
			catchPhrase: 'PHRASYE',
		}
	},
	methods: {
		getRandomPhrase() {
			if (this.groups.length === 0) return (this.catchPhrase = 'Você precisa criar grupos e adicionar frases neles.')

			this.updateGroupSelectedPhrases()

			if (this.groupSelected.phrases.length == 0) {
				return (this.catchPhrase = 'Não há frases para este grupo.')
			}

			let randomNumber = Math.round(Math.random() * (this.groupSelected.phrases.length - 1))

			while (this.groupSelected.phrases[randomNumber] == this.catchPhrase) {
				randomNumber = Math.round(Math.random() * (this.groupSelected.phrases.length - 1))
			}

			this.catchPhrase = this.groupSelected.phrases[randomNumber]
		},
		selectGroup(index) {
			this.groupSelected = { index, phrases: this.groups[index].phrases }
		},
		updateGroupSelectedPhrases() {
			this.groupSelected.phrases = this.groups[this.groupSelected.index].phrases
		},
	},
}
</script>

<style lang="scss">
#GetPhrase {
	width: 100%;

	align-self: center;

	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;

	> h1 {
		font-size: 3.9vw;
		text-align: center;
		padding: 0 48px;
	}
}

.getButton {
	align-self: center;
}

.groupChips {
	display: flex;
	flex-direction: row;
	justify-content: center;
	flex-wrap: wrap;
}
</style>