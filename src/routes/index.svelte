<script context="module">
  import Card from "../components/Card.svelte";

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

  // export let strMatch = "Поиск";
  export let strMatch;

  const renameProp = (prop) => {
    if (prop in alias) {
      return alias[prop];
    } else {
      return prop;
    }
  };

  const search = () => {
    if (strMatch) {
      console.log(strMatch);
    }
  };

  const isMatching = (strMatch, str) => {
    if (strMatch) {
      if (!str.match(new RegExp(strMatch, "i"))) {
        return false;
      }
    }
    return true;
  };
</script>

<input
  class="p-4 max-w-lg mx-auto rounded-xl mb-10"
  placeholder="Поиск"
  bind:value={strMatch}
  type="search"
/>

<div
  class="grid lg:grid-cols-3 md:grid-cols-2 h-full mx-auto px-16 grid-cols-1"
>
  {#await data then users}
    {#each users as user}
      {#if isMatching(strMatch, user.name)}
        <Card>
          <h2 class="text-3xl">{user.name}</h2>
          {#each Object.entries(user) as [key, value]}
            {#if typeof value !== "object"}
              <p>{renameProp(key)}: <b>{value}</b></p>
            {/if}
          {/each}
        </Card>
      {/if}
    {/each}
  {/await}
</div>
