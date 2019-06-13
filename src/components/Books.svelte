<script>
  import BookItem from './BookItem.svelte';
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let books = [];


  const removeBook = e => {
    // console.log(e)
    // console.log(e.detail)
    dispatch("removeBook", e.detail);
  };

  const editBook = e => {
    dispatch("editBook", e.detail);
  };

</script>
<style>
  h1 {
    font-size: 2rem;
  }
</style>

<div>
  {#if books.length === 0}
    <p>No books</p>
    {:else}
    <h1 class="book-header indigo-text text-darken-2">Books</h1>
    <ul className="">
        {#each books as book}
        <div class="">
            <BookItem {...book} on:removeBook={removeBook} on:editBook={editBook}/>
        </div>
        {/each}
    </ul>
    {/if}
  </div>