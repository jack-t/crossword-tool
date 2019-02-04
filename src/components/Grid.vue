<template>
<div class="grid">
	<h2>Grid <small>{{rows.length}} x {{rows[0].length}}</small></h2>
	<label for="lock">Lock Grid</label><input type="checkbox" v-model="locked" />
	<table class="puzzle">
		<tr v-for="row in rows">
			<Square v-for="box in row" :key="box.id" :box="box" v-on:toggle="toggle(box)"/>
		</tr>
	</table>
</div>
</template>

<script>

import Square from './Square'

export default {
	name: 'Grid',
	props: {
		rows: {
			type: Array,
			required: true
		}
	},
	data: function() {return {locked: false};},
	events: ['update_grid'],
	methods: {
		toggle(box) {
			if(!this.locked) {
				this.$set(box, 'black', !box.black); // required because Vue can't react to this kind of event
				this.$emit('update_grid');
			}
		}
	},
	components: {
		Square
	}
}
</script>

<style scoped>
	.box {
		width: 2.75em;
		height: 2.75em;
		border: 1px solid black;
	}

	.number {
		font-size: 7pt;
		position: relative;
		padding-left:4pt;
		top: -2em;
	}

	.letter {
		font-size:18pt;
	}

	.black {
		background-color:black;
	}

	.puzzle {
		border: none;
		border-collapse: collapse;
	}
</style>