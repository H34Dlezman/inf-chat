<script>
export let message;

import {createEventDispatcher} from 'svelte';

const dispatch = createEventDispatcher();

const handleKeyPress = (event) => {
	if (event.key == "Enter") {
		dispatch("click", event)
	}
}
</script>

<div id="ChatBox">
<input type="text" placeholder="me says..." bind:value={message} on:keypress={handleKeyPress}/>
<button on:click>send</button>
</div>
