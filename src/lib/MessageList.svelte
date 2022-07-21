<script>
export let messages
</script>

<ul>
{#each messages as msg}
<li>
{msg}
</li>
{/each}
</ul>
