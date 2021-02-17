<script context="module">
    // Page preload function
    export async function preload(page, session) {
    const res = await this.fetch(`https://jsonplaceholder.typicode.com/users`);
    const data = await res.json();

    return { data };
  }
</script>

<script>
  export let data;

  async function getRandomUser(data) {

      // Get random item from list
      const index = Math.floor(Math.random() * data.length);
      const item = data[index];

      // console.log(item);

      // Return array of properties
      return Object.entries(item);
  }

  let promise = getRandomUser(data);

  function handleClick() {
    promise = getRandomUser(data);
    console.log(promise);
  }
</script>

<!--<button class="rounded bg-indigo-800 hover:bg-indigo-600 text-white py-2 px-4" on:click={handleClick}>-->
<!--    Get random user-->
<!--</button>-->

<main class="grid grid-cols-3 h-auto w-full bg-gray-800 p-12">
    {#await data then users}
        {#each users as user}
            <div class="p-10 max-w-md bg-white rounded-xl space-y-1 m-6">
                <h2 class="text-3xl">{user.name}</h2>
                {#each Object.entries(user) as [key, value]}
                    {#if (typeof(value) !== "object") }
                        <p>{key}: <b>{value}</b></p>
                    {:else}
                        <p>
                            <i>Some object property</i>
                        </p>
                    {/if}
                {/each}
            </div>
        {/each}
    {/await}
</main>
