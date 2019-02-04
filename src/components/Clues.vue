<template>
	<div class="clues">
		<h2>Clues</h2>
		<h3>Across</h3>
		<ul>
			<li v-for="(clue, index) in across" >
				<input type="checkbox" v-model="across_toggle[index]" :id="index" v-on:click="$emit('highlight', 'across', index + 1, !across_toggle[index])" />
				{{clue.number}}
				<input type="text" v-model="clue.text" />
			</li>
		</ul>

		<h3>Down</h3>
		<ul>
			<li v-for="(clue, index) in down" >
				<input type="checkbox" v-model="down_toggle[index]" :id="index" v-on:click="$emit('highlight', 'down', index + 1, !down_toggle[index])" />
				{{clue.number}}
				<input type="text" v-model="clue.text" />
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	props: {
		across: {type: Array, required: true},
		down:   {type: Array, required: true},
	},
	data: function() {
		return {
			across_toggle: [],
			down_toggle: []
		};
	},
	events: ['highlight']
}
</script>