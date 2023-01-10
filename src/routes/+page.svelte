<script lang="ts">
	import Button from '$lib/components/Button.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import NavItem from '$lib/components/NavItem.svelte';

	let isOpen = false;

	let navbarItems: Array<{ title: string; icon: string }> = [
		{ title: 'Data Mapping', icon: 'datamap' },
		{ title: 'Governance Document', icon: 'governance' },
		{ title: 'Employee Awareness', icon: 'employee' },
		{ title: 'Data Processors', icon: 'database' },
		{ title: 'Subject Access Request', icon: 'key' },
		{ title: 'Data Breach Register', icon: 'unlock' }
	];

	let array: Array<{
		department: string;
		subject: string;
		purpose: string;
		legalbasis: string;
	}> = [
		{
			department: 'Human Resources',
			subject: 'Employees',
			purpose: 'Work Permit',
			legalbasis: 'for compliance with a law to which we, the Data Controller is subjected'
		},
		{
			department: 'IT/IS',
			subject: 'Faculty Staff',
			purpose: 'Fingerprinting (BioMetric Data) CRIS 6021',
			legalbasis: 'it is in our legitimate interest'
		},
		{
			department: 'Admission',
			subject: 'Faculty Staff',
			purpose: 'Application',
			legalbasis: 'for the performance of a contract to which the data subject is a party'
		},
		{
			department: 'Admission',
			subject: 'Students',
			purpose: 'Application',
			legalbasis: 'for the performance of a contract to which the data subject is a party'
		}
	];

	let sortBy = { col: 'department', ascending: true };

	$: sort = (column: any) => {
		if (sortBy.col == column) {
			sortBy.ascending = !sortBy.ascending;
		} else {
			sortBy.col = column;
			sortBy.ascending = true;
		}

		let sortModifier = sortBy.ascending ? 1 : -1;

		const sort = (a: any, b: any) =>
			a[column] < b[column] ? -1 * sortModifier : a[column] > b[column] ? 1 * sortModifier : 0;

		array = array.sort(sort);
	};

	function extractUniqueValues<T, K extends keyof T>(arr: T[], property: K): Array<T[K]> {
		return Array.from(new Set(arr.map((item) => item[property])));
	}

	let uniqueDepartments = extractUniqueValues(array, 'department');
	let uniqueSubjects = extractUniqueValues(array, 'subject');
	let uniquePurpose = extractUniqueValues(array, 'purpose');
	let uniqueLegalBasis = extractUniqueValues(array, 'legalbasis');

	let departmentsFilter = new Array(uniqueDepartments.length).fill(true);
	let subjectsFilter = new Array(uniqueSubjects.length).fill(true);
	let purposesFilter = new Array(uniquePurpose.length).fill(true);
	let legalbasisFilter = new Array(uniqueLegalBasis.length).fill(true);

	let filteredArray = array;

	$: filteredArray = array.filter(
		(item) =>
			departmentsFilter[uniqueDepartments.indexOf(item.department)] &&
			subjectsFilter[uniqueSubjects.indexOf(item.subject)] &&
			purposesFilter[uniquePurpose.indexOf(item.purpose)] &&
			legalbasisFilter[uniqueLegalBasis.indexOf(item.legalbasis)]
	);

	function checkAll() {
		departmentsFilter = departmentsFilter.map(() => true);
		subjectsFilter = subjectsFilter.map(() => true);
		purposesFilter = purposesFilter.map(() => true);
		legalbasisFilter = legalbasisFilter.map(() => true);
	}

	function resetFilters() {
		filteredArray = array;
		departmentsFilter.fill(false);
		subjectsFilter.fill(false);
		purposesFilter.fill(false);
		legalbasisFilter.fill(false);
		checkAll();
	}
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
		href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap"
		rel="stylesheet"
	/>
	<link rel="icon" href="/logo.svg" type="image/svg+xml" />

	<title>PDPA International School</title>
</svelte:head>

{#if isOpen}
	<button class="fixed inset-0 bg-black opacity-50 z-10" on:click={() => (isOpen = false)} />
{/if}

<header class=" bg-white w-full">
	<div class="flex flex-row py-3 sm:container mx-4 sm:mx-auto items-center justify-between">
		<div class="flex items-center space-x-4">
			<img src="logo.svg" alt="Logo" class="w-6 sm:w-8" />
			<h1 class="font-semibold text-base sm:text-lg">PDPA / International School</h1>
			<img src="icons/dropdown.svg" alt="dropdown" />
		</div>

		<div class="relative w-8 h-8">
			<img class="rounded-full" src="profielfoto.png" alt="montry" />
		</div>
	</div>
</header>

<main>
	<div class="flex sm:flex-row flex-col py-8 sm:container mx-4 sm:mx-auto items-start">
		<nav class="w-[100%] sm:w-[20%]">
			<ul class="flex flex-col">
				{#each navbarItems as item}
					<NavItem title={item.title} icon={item.icon} />
				{/each}
			</ul>
		</nav>
		<div class="flex flex-col mx-0 xs:mx-14 text-sm w-[100%] sm:w-[80%] py-5 sm:py-0">
			<div class="flex items-center space-x-2 text-[#878787] ">
				<img src="icons/home.svg" alt="home" />
				<span>/</span>
				<p>Current path</p>
			</div>

			<div>
				<div class="flex sm:items-center justify-between flex-col sm:flex-row">
					<p class="font-semibold text-2xl py-4 leading-snug">Data Mapping</p>
					<div class="flex space-x-3 ">
						<Button title="Filter" icon="filter" onClick={() => (isOpen = !isOpen)} />
						<Button title="Export" icon="export" />
						<Button title="Import" icon="import" />
						<Button title="New Data" icon="plus" color="bg-[#009540]" textColor="text-white" />
					</div>
				</div>
			</div>

			<ul class="flex space-x-7 pt-8 border-b ">
				<li
					class="flex cursor-pointer items-center h-full whitespace-nowrap space-x-2 pb-2 border-b-2 border-green-700"
				>
					<img src="icons/datamap.svg" alt="datamap" class="w-4" />
					<p class="font-bold text-sm leading-snug">Data Mapping</p>
				</li>
				<li class="flex cursor-pointer items-center whitespace-nowrap space-x-2 pb-2">
					<img src="icons/collection.svg" alt="datamap" class="w-4" />
					<p class="font-medium text-sm leading-snug text-[#878787]">Collection Sources</p>
				</li>
			</ul>

			<div class="flex space-x-3 py-6">
				<Button title="Edit" icon="edit" color="bg-white" />
				<Button title="Visualize" icon="eye" color="bg-white" />
			</div>

			<div class="relative rounded-md shadow w-full overflow-hidden ">
				{#if filteredArray.length > 0}
					<table class="w-full bg-white table-auto overflow-x-scroll sm:overflow-x-auto block">
						<thead class="text-sm text-[#878787] bg-white border-b">
							<tr>
								<th
									on:click={() => sort('department')}
									class="text-left px-6 py-3 whitespace-nowrap cursor-pointer font-normal"
									>Department</th
								>
								<th
									on:click={() => sort('subject')}
									class="text-left px-6 py-3 whitespace-nowrap cursor-pointer font-normal"
									>Data Subject Type</th
								>
								<th
									on:click={() => sort('purpose')}
									class="text-left px-6 py-3 whitespace-nowrap cursor-pointer font-normal"
									>Processing Purpose</th
								>
								<th
									on:click={() => sort('legalbasis')}
									class="text-left px-6 py-3 whitespace-nowrap cursor-pointer font-normal"
									>Legal Basis</th
								>
								<th />
								<th />
							</tr>
						</thead>
						<tbody>
							{#each filteredArray as row}
								<tr class="bg-white border-b">
									<td class="px-6 py-4 text-gray-900 whitespace-nowrap">{row.department}</td>
									<td class="px-6 py-4 text-gray-900">{row.subject}</td>
									<td class="px-6 py-4 text-gray-900">{row.purpose}</td>
									<td class="px-6 py-4 text-gray-900">{row.legalbasis}</td>
									<td class="w-10"><img src="icons/edit.svg" alt="edit" /></td>
									<td class="w-10"><img src="icons/delete.svg" alt="delete" /></td>
								</tr>
							{/each}
						</tbody>
					</table>

					<div class="py-10 px-5 whitespace-nowrap bg-white">
						<p class="text-[#8c8c8c] text-end text-sm">
							Showing 1-{filteredArray.length} of {filteredArray.length} result{filteredArray.length >
							1
								? 's'
								: ''}
						</p>
					</div>
				{:else}
					<div class="bg-white p-5">
						<p>There were no items found with your filters</p>
					</div>
				{/if}
			</div>
		</div>
	</div>

	<div
		class={`absolute top-0 ${
			!isOpen ? '-right-[100rem]' : 'right-0 '
		}  bg-white min-h-screen shadow justify-between w-full sm:max-w-sm transition-all ease-in-out duration-300 z-50`}
	>
		<div class="flex items-center p-5 justify-between">
			<div class="flex space-x-4 justify-center">
				<img src="icons/filter.svg" alt="filter" class="w-4" />
				<p class="font-bold">Filter</p>
			</div>

			<div class="flex justify-center">
				<Button title="Reset" outline={false} onClick={() => resetFilters()} />
				<button on:click={() => (isOpen = !isOpen)}>
					<img src="icons/close.svg" class="w-5" alt="Close" />
				</button>
			</div>
		</div>

		<div class="flex relative w-full items-center pl-10 border border-l-0 border-r-0">
			<span class="flex absolute left-0 pl-5 bg-transparent rounded text-base text-gray-600 p-2">
				<img src="icons/search.svg" alt="Search icon" />
			</span>
			<input
				type="text"
				placeholder="Search filter"
				class="flex w-full py-5 pl-3 text-[#8c8c8c] flex-none text-sm outline-none"
			/>
		</div>

		<div class="flex flex-col p-5">
			<div class="py-5">
				<h1 class="uppercase text-xs text-[#8c8c8c]">Department</h1>
				<ul>
					{#each uniqueDepartments as department, i}
						<li class="text-sm space-x-4 flex py-1 font-medium">
							<input
								type="checkbox"
								bind:checked={departmentsFilter[i]}
								class="border border-gray-200"
							/>
							<p>{department}</p>
						</li>
					{/each}
				</ul>
			</div>

			<div class="py-5">
				<h1 class="uppercase text-xs text-[#8c8c8c]">Data Subject Type</h1>
				<ul>
					{#each uniqueSubjects as subject, i}
						<li class="text-sm space-x-4 flex py-1 font-medium">
							<input
								type="checkbox"
								bind:checked={subjectsFilter[i]}
								class="border border-gray-200"
							/>
							<p>{subject}</p>
						</li>
					{/each}
				</ul>
			</div>

			<div class="py-5">
				<h1 class="uppercase text-xs text-[#8c8c8c]">Processing Purpose</h1>
				<ul>
					{#each uniquePurpose as purpose, i}
						<li class="text-sm space-x-4 flex py-1 font-medium">
							<input
								type="checkbox"
								bind:checked={purposesFilter[i]}
								class="border border-gray-200"
							/>
							<p>{purpose}</p>
						</li>
					{/each}
				</ul>
			</div>

			<div class="py-5">
				<h1 class="uppercase text-xs text-[#8c8c8c]">Legal Basis</h1>
				<ul>
					{#each uniqueLegalBasis as legalbasis, i}
						<li class="text-sm space-x-4 flex py-1 font-medium">
							<input
								type="checkbox"
								bind:checked={legalbasisFilter[i]}
								class="border border-gray-200"
							/>
							<p>{legalbasis}</p>
						</li>
					{/each}
				</ul>
			</div>
		</div>
	</div>
</main>

<Footer />
