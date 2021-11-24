<template >
	<div id="AddPhrase">
		<div>
			<v-container>
				<v-row class="mb-6 mt-8">
					<v-text-field
						v-model="phrase"
						:disabled="creatingGroup"
						label="Frase"
						filled
						clearable
						color="#537EC5"
						:rules="phraseRules"
						maxlength="110"
						counter
						@update:error="inputErrors.phrase = $event"
					></v-text-field>
				</v-row>
				<v-row class="mb-10" align="center">
					<v-text-field
						v-model="newGroup"
						placeholder="Novo grupo"
						dark
						filled
						clearable
						v-if="creatingGroup"
						:rules="newGroupRules"
						maxlength="40"
						color="#537EC5"
						append-outer-icon="mdi-check-circle"
						@click:append-outer="createGroup"
						@update:error="inputErrors.newGroup = $event"
					></v-text-field>
					<v-autocomplete
						color="#537EC5"
						v-model="group"
						:items="groups"
						:rules="chooseGroupRules"
						item-text="name"
						placeholder="Grupo"
						dark
						filled
						auto-select-first
						no-data-text="Nenhum grupo encontrado"
						v-else
						append-outer-icon="mdi-plus-circle"
						@click:append-outer="createGroup"
						@update:error="inputErrors.group = $event"
					>
					</v-autocomplete>

					<!-- <v-slide-x-reverse-transition mode="out-in">
						<v-icon
							:key="`icon-${creatingGroup}`"
							:color="creatingGroup ? '#F39422' : '#537EC5'"
							@click="createGroup"
							v-text="creatingGroup ? 'mdi-check-circle' : 'mdi-plus-circle'"
							:title="creatingGroup ? 'Salvar grupo' : 'Criar novo grupo'"
							style="margin-left: 4px; padding: 0 8px"
						></v-icon>
					</v-slide-x-reverse-transition> -->
				</v-row>
				<v-row justify="center">
					<v-btn elevation="2" color="#537EC5" class="white--text font-weight-bold" :disabled="creatingGroup" @click="savePhrase">
						<v-icon left dark>mdi-plus</v-icon> Adicionar frase</v-btn
					>
				</v-row>
			</v-container>
		</div>
	</div>
</template>

<script>
export default {
	name: 'AddPhrase',
	data() {
		return {
			phrase: '',
			group: '',
			newGroup: '',
			creatingGroup: false,
			phraseRules: [value => !!value || 'Obrigatorio', value => (value && value.length >= 3) || 'Minimo 3 caracteres'],
			chooseGroupRules: [value => !!value || 'Obrigatorio'],
			newGroupRules: [
				value => (value && value.length >= 1 && value.length <= 3 ? 'Mínimo 4 caracteres' : true),
				value => (this.groups.find(group => group.name === value) ? 'Grupo já existe' : true),
			],
			inputErrors: {
				phrase: false,
				group: false,
				newGroup: false,
			},
		}
	},
	props: {
		groups: {
			type: Array,
			required: true,
		},
	},
	methods: {
		createGroup() {
			if (this.inputErrors.newGroup) return

			this.creatingGroup = !this.creatingGroup

			if (!this.creatingGroup) {
				if (!this.newGroup || this.groups.find(group => group.name == this.newGroup)) return
				this.groups.push({ name: this.newGroup, phrases: [] })
				this.newGroup = ''
			}
		},
		savePhrase() {
			if (this.inputErrors.phrase || this.inputErrors.group) return

			this.$emit('addPhrase', { group: this.group, phrase: this.phrase })
			this.phrase = ''
		},
	},
}
</script>

<style lang="scss">
#AddPhrase {
	width: 100%;

	align-self: center;

	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;

	> div {
		width: 80%;
		max-width: 720px;
	}
}
</style>