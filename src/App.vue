<template>
	<v-app id="App">
		<div id="Modes">
			<p class="codedBy">Coded by <a href="http://p0sseid0n.site" target="_blank" rel="noopener noreferrer">P0sseid0n</a></p>
			<ChangeMode :mode="mode" @getMode="mode = 'get'" @addMode="mode = 'add'" />

			<EditGroup
				v-if="mode == 'edit'"
				:group="this.groups[this.editingGroup]"
				@updateGroup="updateGroupName"
				@deletePhrase="deletePhrase"
				@updatePhrase="updatePhrase"
				:groups="groups"
			/>
			<GetPhrase @editMode="editMode" @deleteGroup="deleteGroup" :groups="groups" v-else-if="mode == 'get'" />
			<AddPhrase @addPhrase="addPhrase" :groups="groups" v-else-if="mode == 'add'" />
		</div>
	</v-app>
</template>

<script>
import EditGroup from './components/EditGroup'
import AddPhrase from './components/AddPhrase'
import GetPhrase from './components/GetPhrase'

import ChangeMode from './components/ChangeMode'

export default {
	name: 'App',
	components: { EditGroup, AddPhrase, GetPhrase, ChangeMode },
	data() {
		return {
			groups: [],
			mode: 'get',
			editingGroup: null,
		}
	},
	methods: {
		addPhrase({ group: groupName, phrase }) {
			this.groups = this.groups.map(group => {
				if (group.name === groupName) group.phrases.push(phrase)
				return group
			})
			this.saveOnStorage()
		},
		editMode(groupIndex) {
			this.editingGroup = groupIndex
			this.mode = 'edit'
		},
		updateGroupName(groupName) {
			this.groups[this.editingGroup].name = groupName
			this.saveOnStorage()
		},
		deleteGroup(groupIndex) {
			this.groups.splice(groupIndex, 1)
			this.saveOnStorage()
		},
		updatePhrase({ phrase, index }) {
			this.groups[this.editingGroup].phrases[index] = phrase
			this.saveOnStorage()
		},
		deletePhrase(phraseIndex) {
			this.groups[this.editingGroup].phrases.splice(phraseIndex, 1)
			this.saveOnStorage()
		},
		saveOnStorage() {
			localStorage.setItem('groups', JSON.stringify(this.groups))
		},
	},

	mounted() {
		this.groups = JSON.parse(localStorage.getItem('groups')) || [
			{
				name: 'Melhorar o humor',
				phrases: [
					'Acredite: tudo tem seu tempo.',
					'Você é forte, você é capaz, você pode.',
					'Amanhã será um dia melhor.',
					'O melhor está por vir. Acredite!',
					'Às vezes, uma coisa boa acaba para que outra melhor comece.',
					'O segredo é um só: acreditar que tudo vai dar certo, porque vai.',
					'São os caminhos difíceis que nos levam aos finais felizes.',
					'Persista! Se tudo fosse fácil, qualquer um conseguiria.',
					'Se não puder fazer tudo, faça tudo que puder.',
					'Por maior que seja, não há obstáculo que não possa ser superado.',
					'Esqueça, levante a cabeça. Siga em frente. Amanhã é um novo dia.',
				],
			},
		]
	},
}
</script> 

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;500;600;700&display=swap');

* {
	font-family: 'Quicksand', sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

html {
	overflow-y: unset;
}

html,
body,
#App {
	background-color: #010038;
}

#Modes {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	width: 100%;
	height: 100vh;
}

.codedBy {
	position: absolute;
	top: 24px;

	a {
		color: white !important;
		font-weight: bold;

		text-decoration: underline;

		&:hover {
			color: #ffc107 !important;
			text-decoration: none;
		}
	}
}
</style>
