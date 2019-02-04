<template>
	<td :class="{black: box.black, highlight: box.highlight}" v-on:dblclick="toggle" class="box">
		<span class="number" v-if="!box.black">{{box.number}}</span>
		<span class="letter" v-if="!box.black">
			<input type="text" v-model="box.letter" maxlength=1 />
		</span>
	</td>
</template>

<script>
export default {
	name: 'Square',
	props: {
		box: {
			type: Object,
			required: true
		}
	},
	events: ['toggle'],
	methods: {
		toggle() {
			this.$emit('toggle');
		}
	}
}
</script>

<style>
	.box {
		position: relative; /* required for the numbers to be absolutely positioned */
	}
	.number {
		position: absolute;

		top: 0.2em;
		left: 0.2em;
		margin-left:0.2em;
		font-size: 7pt;
	}
	.letter input[type=text] {
		font-size:18pt;
		text-align: center;
		text-transform: capitalize;
		
		border: 0;
		width: 80%;
		height: 80%;
		margin-left:0.15em;
		margin-top:0.15em;
	}
	.letter input[type=text]:focus {
		outline: none;
	}
	.highlight {
		background-color: #ff0;
	}
	.black {
		background-color: black;
	}
</style>