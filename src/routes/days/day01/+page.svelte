<script lang="ts">
	// types
	type Person = {
		name: string;
		tally: number;
	};

	// raw data
	const dataJson = `[
		{ "name": "Emma", "tally": 32 },
		{ "name": "Ethan", "tally": 14 },
		{ "name": "Isabella", "tally": 70 },
		{ "name": "Jayden", "tally": -16 },
		{ "name": "Isabella", "tally": -59 },
		{ "name": "Noah", "tally": 19 },
		{ "name": "Mia", "tally": -37 },
		{ "name": "Will", "tally": -20 },
		{ "name": "Sam", "tally": -91 },
		{ "name": "Brittney", "tally": -98 }
	]`;

	// parse data
	const dataParsed: Person[] = JSON.parse(dataJson);

	// state
	const data = $state(dataParsed);

	// test

	// functions
	function addPerson(name: string, tally: number) {
		data.push({ name, tally });
		$inspect(data);
	}

	addPerson("John", 100);
</script>

<template lang="pug">
	main.px-4.py-8
		//- create simple grid
		.grid.grid-cols-3
			//- loop through data -- each datum is a row 
			+each('data as datum, rowIndex')
				//- derive status from tally
				+const('status = datum.tally > 0 ? "nice" : "naughty"')

				//- whip up an array of keys for each datum plus status
				+const('keys = [...Object.keys(datum), "status"]')

				//- loop through keys -- each key is a cell
				+each('keys as key, cellIndex')
					+const('up = "ArrowUp"')
					+const('dn = "ArrowDown"')

					//- each cell is a button
					button(
						class=`
							p-1
							whitespace-nowrap
							{rowIndex === 0 ? 'border-y' : 'border-b'}
							{cellIndex === 2 ? 'border-x' : 'border-l'}`,
						contentEditable="true",
						disabled!="{ cellIndex !== 1 }"
					) { datum[key] ? datum[key] : status ?? "" }</template>
