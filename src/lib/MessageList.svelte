<script>
export let messages
export let loaded
</script>

<ul id="MessageList">

{#each messages as msg}
<li>
<p>{msg}</p>
</li>
{/each}

{#if messages.length == 0 && loaded}
<li>
	<p style="color: red;">BE DA FURST</p>
</li>
{/if}

</ul>
