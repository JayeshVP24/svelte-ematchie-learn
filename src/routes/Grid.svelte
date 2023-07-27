<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Square from './Square.svelte';

	export let grid: string[];
	export let found: string[];

    const dispatch = createEventDispatcher()

	let a: number = -1;
	let b: number = -1;

    let reset_timeout: number

    export function newGame() {
        a = -1
        b = -1
    }
</script>

<div class="grid">
	{#each grid as emoji, i}
		<Square
			{emoji}
			on:click={() => {
                clearTimeout(reset_timeout)
				if (a === -1 && b === -1) a = i
                else if(b === -1) {
                    b = i
                    if(grid[a] === grid[b]) {
                        // found pair
                        dispatch("found", {
                            emoji
                        })
                    } else {
                        // incorect
                        reset_timeout = setTimeout(() => {
                            a = b = -1
                        }, 1000)
                    }
                } else {
                    b = -1;
                    a = i;
                }
			}}
			selected={a === i || b === i}
            found={found.includes(emoji)}
            group={grid.indexOf(emoji) === i ? 'a' : 'b'}
		/>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(var(--size), 1fr);
		grid-template-rows: repeat(var(--size), 1fr);
		height: 100%;
		grid-gap: 1em;
        perspective: 100vw;
	}
</style>
