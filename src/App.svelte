<script>
	import data from './qa.json';
	const urlParams = new URLSearchParams(window.location.search);
	let index = urlParams.get('q') - 1;
	if (index == '') {
		index = 0;
	}

	function initializeStates() {
		for (let i = 0; i < data[index].answers.length; i++) {
			data[index].answers[i].clicked = false;
		}
	}

	let team1 = 0;
	let team2 = 0;
	let currentTeam = true;

	function handleFlip(answerNumber) {
		data[index].answers[answerNumber - 1].clicked = !data[index].answers[answerNumber - 1].clicked;
		let flipped = data[index].answers[answerNumber - 1].clicked;
		if (currentTeam && flipped) {
			team1 += data[index].answers[answerNumber - 1].points;
		}
		else if (currentTeam && !flipped) {
			team1 -= data[index].answers[answerNumber - 1].points;
		}
		else if (!currentTeam && flipped) {
			team2 += data[index].answers[answerNumber - 1].points;
		}
		else if (!currentTeam && !flipped) {
			team2 -= data[index].answers[answerNumber - 1].points;
		}
	}
	function handleNext() {
		index++;
		initializeStates();
	}
	function switchTeam() {
		currentTeam = !currentTeam;
	}
</script>

<main>
	<img on:click={handleNext} alt="family feud logo" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/Logo_of_Family_Feud.png/250px-Logo_of_Family_Feud.png">

<!--
	<h1>{ data[index].question }</h1>
-->
	<h1>{ index + 1 }</h1>
	<div class="answer-board">
		{#each data[index].answers as answer}
			<div class="answer" on:click={handleFlip(answer.number)}>
				{#if answer.clicked}
					{ answer.solution } - {answer.points }
				{:else}
					{ answer.number }
				{/if}
			</div>
		{/each}
	</div>
	<div on:click={ switchTeam }>
		{#if currentTeam}
			<p style="color: salmon;">Team One: {team1}</p>
			<p>Team Two: {team2}</p>
		{:else}
			<p>Team One: {team1}</p>
			<p style="color: salmon;">Team Two: {team2}</p>
		{/if}
	</div>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Oswald&display=swap');
	:root {
		background-color: blue;
	}
	main {
		font-family: 'Oswald';
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		color: white;
	}

	h1 {
		text-transform: uppercase;
		font-size: 2em;
	}

	.answer-board {
		display: flex;
		flex-flow: row wrap;
	}

	.answer {
		flex: 40%;
		font-size: 1.5em;
		background-color: cornflowerblue;
		color: white;
		margin: 1%;
		padding: 1%;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>