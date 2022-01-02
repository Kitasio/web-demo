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
				css: t => `
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
		{ id: 6, done: false, description: 'Закрыть все вкладки в браузере' },
	];

	let uid = todos.length + 1;

	function add(input) {
		// const todo = {
		// 	id: uid++,
		// 	done: false,
		// 	description: input.value
		// };

		// todos = [todo, ...todos];
		// input.value = '';
	}

	function remove(todo) {
		// todos = todos.filter(t => t !== todo);
	}
</script>

<div class='board'>
	<input
		class="new-todo"
		placeholder="Че сделать?"
		on:keydown="{event => event.key === 'Enter' && add(event.target)}"
	>

	<div class='left'>
		<h2>ТУДУ</h2>
		{#each todos.filter(t => !t.done) as todo (todo.id)}
			<label
			>
				<!-- in:receive="{{key: todo.id}}"
				out:send="{{key: todo.id}}"
				animate:flip -->
				<input type=checkbox>
				{todo.description}
				<button on:click="{() => remove(todo)}">x</button>
			</label>
		{/each}
	</div>

	<div class='right'>
		<h2>ДАН</h2>
		{#each todos.filter(t => t.done) as todo (todo.id)}
			<label
			>
				<input type=checkbox>
				{todo.description}
				<button on:click="{() => remove(todo)}">x</button>
			</label>
		{/each}
	</div>
</div>
<img src="/bg.svg" alt="">

<a href="/css">Далее</a>