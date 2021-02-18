<script context="module">
  // Page preload function
  export async function preload(page, session) {
    const res = await this.fetch(`https://jsonplaceholder.typicode.com/users`);
    const data = await res.json();

    const resAlias = await this.fetch(`client/alias.json`);
    const alias = await resAlias.json();

    return { data, alias };
  }
</script>

<script>
  export let data;
  export let alias;

  const renameProp = (prop) => {
    if (prop in alias) {
      return alias[prop];
    } else {
      return prop;
    }
  };
</script>

<main class="h-auto w-full bg-gray-800">
<!--<input class="m-8 p-2 max-w-md rounded-md" value="Поиск" type="search">-->
<div class="grid lg:grid-cols-3 md:grid-cols-2 mx-auto p-16 grid-cols-1">
    {#await data then users}

        {#each users as user}
            <div class="p-10 bg-gray-100 rounded-xl space-y-1 m-6">
                <h2 class="text-3xl">{user.name}</h2>
                {#each Object.entries(user) as [key, value]}
                    {#if typeof value !== "object"}
                        <p>{renameProp(key)}: <b>{value}</b> </p>
                    {:else}
                        <!--            <p>-->
                        <!--              <i>Some object property</i>-->
                        <!--            </p>-->
                    {/if}
                {/each}
            </div>
        {/each}

    {/await}
</div>

</main>
