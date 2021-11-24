<template>
	<div id="EditGroup">
		<div id="editForm" class="mb-8">
			<v-text-field
				v-model="newGroupName"
				label="Nome do grupo"
				filled
				color="#537EC5"
				maxlength="40"
				:rules="groupRules"
				counter
				class="mt-8"
				append-icon="mdi-refresh"
				@click:append="resetGroupName"
			></v-text-field>
			<v-btn elevation="2" color="#537EC5" class="white--text font-weight-bold" @click="updateGroup">
				<v-icon left dark>mdi-plus</v-icon>Salvar</v-btn
			>
		</div>
		<div id="phraseList">
			<v-simple-table class="phraseTable" height="240">
				<template v-slot:default>
					<thead>
						<tr>
							<th class="text-left">Frase</th>
							<th></th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(phrase, index) in group.phrases" :key="phrase + index" class="phrasesTr">
							<td class="pr-3">
								<v-text-field
									v-if="editingPhrase == index"
									v-model="phraseToSave"
									full-width
									label="Frase"
									dense
									filled
									color="#537EC5"
									class="mt-5"
									counter
									maxlength="110"
									:rules="phraseRules"
								></v-text-field>
								<span v-else>{{ phrase }}</span>
							</td>
							<td style="width: 90px" class="pl-0">
								<span v-if="editingPhrase == index">
									<v-btn plain icon @click="editMode(index)">
										<v-icon>mdi-check-bold</v-icon>
									</v-btn>
									<v-btn plain icon @click="resetPhrase">
										<v-icon>mdi-refresh</v-icon>
									</v-btn>
								</span>
								<span v-else>
									<v-btn plain icon @click="editMode(index)">
										<v-icon>mdi-pencil</v-icon>
									</v-btn>
									<v-btn plain icon @click="deletePhrase(index)"><v-icon>mdi-delete</v-icon></v-btn>
								</span>
							</td>
						</tr>
					</tbody>
				</template>
			</v-simple-table>
		</div>
	</div>
</template>

<script>
export default {
	name: 'EditGroup',
	props: {
		group: {
			type: Object,
			required: true,
		},
		groups: {
			type: Array,
			required: false,
		},
	},
	data() {
		return {
			groupName: this.group.name,
			newGroupName: this.group.name,
			editingPhrase: null,
			phraseToSave: '',
			phraseRules: [value => !!value || 'Obrigatorio', value => (value && value.length >= 3) || 'Minimo 3 caracteres'],
			groupRules: [
				value => (value && value.length >= 1 && value.length <= 3 ? 'Mínimo 4 caracteres' : true),
				value => (value != this.groupName && this.groups.find(group => group.name === value) ? 'Grupo já existe' : true),
			],
		}
	},
	methods: {
		updateGroup() {
			this.$emit('updateGroup', this.newGroupName)
			this.groupName = this.newGroupName
		},
		resetGroupName() {
			this.newGroupName = this.groupName
		},
		resetPhrase() {
			this.phraseToSave = this.group.phrases[this.editingPhrase]
		},
		deletePhrase(phrase) {
			this.$emit('deletePhrase', phrase)
		},
		editMode(phraseIndex) {
			if (this.editingPhrase == null) {
				this.editingPhrase = phraseIndex
				this.phraseToSave = this.group.phrases[phraseIndex]
			} else {
				this.$emit('updatePhrase', {
					phrase: this.phraseToSave,
					index: this.editingPhrase,
				})

				if (phraseIndex == this.editingPhrase) {
					this.editingPhrase = null
				} else {
					this.phraseToSave = this.group.phrases[phraseIndex]
					this.editingPhrase = phraseIndex
				}
			}
		},
	},
}
</script>

<style lang="scss">
#EditGroup {
	width: 100%;

	align-self: center;

	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;

	> div {
		width: 80%;
		max-width: 720px;

		display: flex;
		flex-direction: column;
		align-items: center;

		> .v-input {
			width: 100%;
		}

		> button {
			align-self: center;
		}
	}
}

.phraseTable {
	width: 100%;
}

.phrasesTr {
	// display: table-column;
}
</style>