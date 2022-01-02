<script>
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';
	import { flip } from 'svelte/animate';

	const [send, receive] = crossfade({
		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: (t) => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let todos = [
		{ id: 1, done: false, description: 'Покормить синиц' },
		{ id: 2, done: false, description: 'Налить воды Лангуру' },
		{ id: 3, done: false, description: 'Помыть чашку от кофе' },
		{ id: 4, done: false, description: 'Найти смысл жизни' },
		{ id: 5, done: false, description: 'Вынести мусор' },
		{ id: 6, done: false, description: 'Закрыть все вкладки' }
	];

	let uid = todos.length + 1;

	function add(input) {
		const todo = {
			id: uid++,
			done: false,
			description: input.value
		};

		todos = [todo, ...todos];
		input.value = '';
	}

	function remove(todo) {
		todos = todos.filter((t) => t !== todo);
	}
</script>

<div class="box w-full bg-cover">
	<div class="board">
		<input
			class="new-todo p-2"
			placeholder="Че сделать?"
			on:keydown={(event) => event.key === 'Enter' && add(event.target)}
		/>

		<div class="left">
			<h2>ТУДУ</h2>
			{#each todos.filter((t) => !t.done) as todo (todo.id)}
				<label
				>
					<input type="checkbox" bind:checked={todo.done} />
					<span class="transition duration-200 hover:text-cyan-400">{todo.description}</span>
					<button on:click={() => remove(todo)}>x</button>
				</label>
			{/each}
		</div>

		<div class="right">
			<h2>ДАН</h2>
			{#each todos.filter((t) => t.done) as todo (todo.id)}
				<label
				>
					<input type="checkbox" bind:checked={todo.done} />
					<span class="transition duration-200 hover:text-cyan-400">{todo.description}</span>
					<button on:click={() => remove(todo)}>x</button>
				</label>
			{/each}
		</div>
	</div>
    <div class="flex w-full items-center justify-center pt-20">
        <a class="border-2 border-sky-500 rounded py-2 text-sky-700 px-5 hover:scale-105 transition duration-200 hover:bg-sky-500 hover:text-white" href="/final">Далее</a>
    </div>
</div>

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;

	.box {
		background-image: url('/bg.svg');
		background-size: cover;
		height: 100vh;
	}
	.new-todo {
		font-size: 1.4em;
		width: 100%;
		margin: 2em 0 1em 0;
	}

	.board {
		max-width: 36em;
		margin: 0 auto;
	}

	.left,
	.right {
		float: left;
		width: 50%;
		padding: 0 1em 0 0;
		box-sizing: border-box;
	}

	h2 {
		font-size: 2em;
		font-weight: 200;
		user-select: none;
	}

	label {
		top: 0;
		left: 0;
		display: block;
		font-size: 1em;
		line-height: 1;
		padding: 0.5em;
		margin: 0 auto 0.5em auto;
		border-radius: 2px;
		background-color: #eee;
		user-select: none;
	}
	/* label:hover {
		--tw-scale-x: 1.05;
		--tw-scale-y: 1.05;
		transform: var(--tw-transform);
		transition-duration: 300ms;
	} */

	input {
		margin: 0;
		padding: 5px;
	}

	.right label {
		background-color: rgb(180, 240, 100);
	}

	button {
		float: right;
		height: 1em;
		box-sizing: border-box;
		padding: 0 0.5em;
		line-height: 1;
		background-color: transparent;
		border: none;
		color: rgb(170, 30, 30);
		opacity: 0;
		transition: opacity 0.2s;
	}

	label:hover button {
		opacity: 1;
	}
</style>
