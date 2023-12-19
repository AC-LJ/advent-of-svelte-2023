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
	function onButtonClick(event: PointerEvent, name?: string, tally?: number) {
		name = name ?? "-";
		tally = tally ?? 0;
		console.log({ event, name, tally });
		console.log({ data });
		addPerson(name, tally);
	}

	function addPerson(name: string, tally: number) {
		data.push({ name, tally });
		// console.log({ name, tally });
		// console.log(data);
		// $inspect(data);
	}

	function scrubTally(event: InputEvent) {
		const el: HTMLButtonElement = event.target as HTMLButtonElement;
		const text = el.textContent ?? "";
		const scrubbed = text
			.replace(/[^0-9-]/g, "")
			.replace("--", "-")
			.replace(/(\d+?)-/, "$1");
		console.log(scrubbed);
		el.textContent = scrubbed;
	}

	addPerson("John", 100);
</script>

<template lang="pug">
	main.px-4.py-8.grid.grid-cols-1.gap-y-8
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
						disabled!="{ cellIndex === 2 }",
						on:blur!=`{
							(e) =>
								data[rowIndex][key] = 
									e?.target.textContent ? (Number(e.target.textContent ?? 0)) : 0
						}`,
						on:input!="{ cellIndex === 1 ? scrubTally : null }",
						on:keydown!=`{
							(e) => 
								e.key === up && e.preventDefault && data[rowIndex][key]++ ||
								e.key === dn && e.preventDefault && data[rowIndex][key]--
							}`
					) { datum[key] !== undefined ? datum[key] : status ?? "" }

		button(
			class=`
			border
			border-white
			max-w-fit
			px-3
			py-2
			rounded-lg
			hover:bg-blue-100/20
		`,
			on:click!="{ onButtonClick }"
		) Add Person</template>
