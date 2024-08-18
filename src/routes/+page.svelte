<script lang="ts">
	import Hero from '$lib/components/Hero.svelte';
	import { writable, derived } from 'svelte/store';
	import { onMount } from 'svelte';
	import _ from 'lodash';
	import MiniSearch from 'minisearch';

	/////////////////
	// General Utils
	/////////////////

	function formatDate(date: Date): string {
		return date.toISOString().split('T')[0];
	}

	function addDaysToDate(date: Date, days: number): Date {
		// Add days to a date, without modifying the input date
		const newDate = new Date(date);
		newDate.setDate(date.getDate() + days);
		return newDate;
	}

	/////////////////
	// Library/Store
	/////////////////

	interface LibraryEvent {
		id: number;
		title: string;
		date: string;
		link: string;
		location: string;
		time: string;
		summary: string;
		day: string;
		month: string;
		formattedDate: string;
	}

	type Filter = {
		locations: string[]; // Array of strings of selected locations. If empty, everything is selected.
		searchQuery: string;
		startDate: string;
		endDate: string;
	};

	type LocationsCount = Record<string, number>;

	let miniSearch: MiniSearch;

	const events = writable<LibraryEvent[]>([]);

	const defaultFilterValues = {
		locations: [],
		searchQuery: '',
		startDate: formatDate(new Date()),
		endDate: formatDate(addDaysToDate(new Date(), 60))
	};
	const filter = writable<Filter>(_.clone(defaultFilterValues));

	filter.subscribe((value) => {
		// Make sure that the start date is before the end date
		if (value.startDate > value.endDate) {
			filter.update((f) => ({ ...f, endDate: value.startDate }));
		}
	});

	const filteredEvents = derived([filter, events], ([$filter, $events]) => {
		const searchOptions = { fuzzy: 0.2, prefix: true };
		const { locations, searchQuery, startDate, endDate } = $filter;

		let filtered = $events;

		// Filter by searchQuery
		if (searchQuery !== '') {
			const searchResults = miniSearch.search(searchQuery, searchOptions);
			const resultIdsSet = new Set(searchResults.map((e) => e.id));
			filtered = _.filter(filtered, (event) => resultIdsSet.has(event.id));
		}

		// Filter by date
		filtered = _.filter(filtered, (event) => {
			const eventDate = new Date(event.date);
			const startDateDate = new Date(startDate);
			const endDateDate = new Date(endDate);
			return startDateDate <= eventDate && eventDate < addDaysToDate(endDateDate, 1);
		});

		// Filter by selected locations
		if (locations.length !== 0) {
			filtered = _.filter(filtered, (event) => _.includes(locations, event.location));
		}

		return filtered;
	});

	const locations = derived(
		events,
		($events: LibraryEvent[]): LocationsCount => _.countBy($events, 'location')
	);

	// https://svelte.dev/repl/2d6e421ca6ad4103a9027c5e8ed4240f?version=3.48.0

	//////////////////////////////////// ////////////
	// CHANGE HANDLERS FILE/COMPONENT SPECIFIC THINGS
	//////////////////////////////////// ////////////

	interface RawLibraryEvent {
		title: string;
		date: string;
		link: string;
		location: string;
		time: string;
		summary: string;
	}

	function handleCheckboxChange(event: Event) {
		const target = event.target as HTMLInputElement;
		if (target.checked) {
			$filter.locations = [...$filter.locations, target.value];
		} else {
			$filter.locations = $filter.locations.filter((location) => location !== target.value);
		}
	}
	function clearFilters() {
		// Reset the search input
		const texts = document.querySelectorAll("input[type='text']");
		for (const text of texts) {
			text.value = '';
		}

		// Uncheck all the boxes
		const boxes = document.querySelectorAll("input[type='checkbox']");
		for (const box of boxes) {
			box.checked = false;
		}

		// Set the filter to the default
		console.log('defaults', defaultFilterValues);
		filter.update(() => _.clone(defaultFilterValues));
	}

	async function loadEvents() {
		// Load in the events
		const rawEventsResponse = await fetch('/events.json');

		if (!rawEventsResponse.ok) {
			console.error('Failed to fetch events');
			return;
		}

		const rawEvents: RawLibraryEvent[] = await rawEventsResponse.json();

		const eventsArray = rawEvents.map((rawEvent, idx) => {
			const date = new Date(rawEvent.date);
			const month = date.toLocaleString('en-US', { month: 'short' });
			const day = date.getDate().toString();

			const formattedDate = date.toLocaleDateString('en-us', {
				month: 'long',
				day: 'numeric',
				year: 'numeric'
			});

			return {
				...rawEvent,
				id: idx,
				month,
				day,
				formattedDate
			};
		});
		events.set(eventsArray);

		// Initialize MiniSearch
		miniSearch = new MiniSearch({ fields: ['title'], storeFields: ['title'] });
		miniSearch.addAll($events);
	}

	onMount(() => {
		loadEvents();
	});
</script>



<Hero />
<div class="container">
	<section class="section">
		<h2 class="heading-2 ml--1 mt-5rem mb-5rem">Multnomah Count Library Events</h2>
		<div class="grid-2col">
			<div class="event-filters | flow mt-dot25rem mb-5rem">
				<h3 class="heading-3">Filters</h3>
				<fieldset class="mb-1rem">
					<legend>Search</legend>
					<input type="text" bind:value={$filter.searchQuery} placeholder="Search events..." />
				</fieldset>
				<fieldset>
					<legend>Date</legend>
					<div class="date-label-and-input">
						<label for="start-date">From:</label>
						<input type="date" id="start-date" bind:value={$filter.startDate} />
					</div>
					<div class="date-label-and-input">
						<label for="end-date">Until:</label>
						<input type="date" id="end-date" min={$filter.startDate} bind:value={$filter.endDate} />
					</div>
				</fieldset>
				<fieldset>
					<legend>Location</legend>
					{#each Object.entries($locations) as [location, count]}
						<div class="filter-group">
							<input
								type="checkbox"
								id={location}
								name="data-location"
								value={location}
								on:change={(e) => handleCheckboxChange(e)}
							/>
							<label for={location}>{location} ({count})</label>
						</div>
					{/each}
				</fieldset>
				<!-- Support this button -->
				<button class="button | mt-1rem" data-type="outline" on:click={clearFilters}
					>Clear filters</button
				>
				<hr />
				<fieldset>
					<legend>Ages</legend>
					<p>Coming soon</p>
				</fieldset>
				<fieldset>
					<legend>Event Type</legend>
					<p>Coming soon</p>
				</fieldset>
				<fieldset>
					<legend>Language & Culture</legend>
					<p>Coming soon</p>
				</fieldset>
			</div>
			<div>
				{#each $filteredEvents as event}
					<div class="event" data-location="Hollywood Library">
						<div class="date-stamp">
							<div class="date-stamp__month" aria-hidden="true">
								{event.month}
							</div>
							<div class="date-stamp__day" aria-hidden="true">{event.day}</div>
						</div>
						<div class="event__content flow">
							<h2 class="heading-3">{event.title}</h2>
							<div class="event__whereandwhen">
								<p>{event.formattedDate}, {event.time}</p>
								<a class="link" href="#{event.location}">{event.location}</a>
							</div>
							<p>
								{event.summary}
							</p>
							<a class="button" href={event.link}>Register for event</a>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>
</div>

<style>
	:root {
		--color-primary-200: oklch(90.62% 0.049 219.65);
		--color-primary-400: oklch(85.62% 0.049 219.65);
		--color-primary-600: oklch(70.62% 0.049 219.65);
		--color-primary-800: oklch(50% 0.049 219.65);
		--color-primary-900: oklch(40.62% 0.049 219.65);
		--color-gray-900: rgb(30, 30, 30);
	}

	h2,
	h3 {
		margin-block: 0;
	}

	.heading-2 {
		font-size: 3rem;
		line-height: 1.2;
		text-wrap: balance;

		font-family: 'Playfair Display', serif;
		font-optical-sizing: auto;
		font-weight: 600;
		font-style: normal;
	}

	.heading-3 {
		font-size: 2.5rem;
		line-height: 1.2;
		text-wrap: balance;
		margin-bottom: 1.25rem;

		font-family: 'Playfair Display', serif;
		font-optical-sizing: auto;
		font-weight: 600;
		font-style: normal;
	}

	.event {
		max-width: 60ch;
		margin-bottom: 3rem;

		display: grid;
		gap: 1.5rem;
		grid-template-columns: 76px auto;

		@media (max-width: 600px) {
			grid-template-columns: auto;
		}
	}

	.event__whereandwhen {
		font-weight: 500;
	}

	.button {
		display: inline-flex;
		justify-content: center;
		cursor: pointer;
		text-decoration: none;
		line-height: 1;

		padding: 1rem 1.25rem;
		border: 1px solid black;
		border-radius: 4px;
		transition: 275ms ease;

		background-color: var(--color-primary-400);
		color: black;

		&:is(:hover, :focus) {
			background-color: var(--color-primary-600);
			outline-color: var(--color-gray-900);
		}
	}

	.button[data-type='outline'] {
		background-color: transparent;
		color: inherit;

		&:is(:hover) {
			background-color: var(--color-primary-400);
		}

		&:is(:focus) {
			outline: var(--color-primary-400);
		}
	}

	.link {
		cursor: pointer;
		text-decoration: none;
		color: var(--color-primary-800);

		&:is(:hover, :focus) {
			text-decoration: underline;
			color: var(--color-primary-900);
		}
	}

	.event-filters {
		padding-right: 2rem;
	}

	input[type='text'] {
		width: 100%;
	}

	input[type='checkbox'] {
		width: 1.25rem;
		height: 1.25rem;
	}

	.date-label-and-input {
		display: grid;
		align-items: center;
		gap: 1rem;
		grid-template-columns: 8ch auto;
		width: 100%;
		margin-top: 0.375rem;
	}

	.filter-group {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		margin-bottom: 0.25rem;
	}

	.date-stamp {
		display: grid;
		gap: 0;

		grid-template-rows: 30px 46px;
		grid-template-columns: 1fr;
		height: 72px;
		width: 76px;

		border: 1px solid black;
		border-radius: 4%;

		margin-top: 0.75rem;

		& > .date-stamp__month {
			font-size: 1.125rem;
			color: white;
			text-align: center;
			background: var(--color-primary-900);
			display: grid;
			place-content: center;
		}

		& > .date-stamp__day {
			text-align: center;
			margin-block: auto;
		}

		@media (max-width: 600px) {
			display: none;
		}
	}

	.mt-dot25rem {
		margin-top: 0.25rem;
	}

	.mt-1rem {
		margin-top: 1rem;
	}

	/* 1,2,3,5,8,13 for fibonacci */
	.mb-1rem {
		margin-bottom: 1rem;
	}
	.mb-5rem {
		margin-bottom: 5rem;
	}
	.ml--1 {
		margin-left: -0.25rem;
	}
	.grid-2col {
		display: grid;
		gap: 3rem;
		grid-template-columns: 32ch auto;
	}

	@media (max-width: 1000px) {
		.grid-2col {
			display: flex;
			gap: 5rem;
			flex-direction: column;
		}
	}
</style>
