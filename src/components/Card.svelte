<script>
  export let userData;
  export let renameProp;
</script>

<style>
  .user-card {
    @apply p-10 bg-gray-200 rounded-md space-y-1 m-6;
  }
</style>

<div class="user-card">
  <h2 class="text-3xl">{userData.name}</h2>
  {#each Object.entries(userData) as [key, value]}
    {#if typeof value !== "object"}
      <p>{renameProp(key)}:
        {#if key==="website"} <b><a href="http://{value}" target="_blank">{value}</a></b>
        {:else if key==="email"} <b><a href="mailto:{value}" target="_blank">{value}</a></b>
        {:else} <b>{value}</b>
        {/if}
      </p>
    {/if}
  {/each}
</div>
