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
<input placeholder="me says... (🐣 @akaNAM)" bind:value={message} on:keypress={handleKeyPress} enterkeyhint="send" type="text"/>
<button on:click disabled={message.length==0}>send</button>
</div>
