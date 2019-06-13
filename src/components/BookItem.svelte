<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let title;
  export let author;
  export let id;
  const onDelete = () => dispatch("removeBook", id);
  const onEdit = () =>
    dispatch("editBook", {
      title,
      author,
      id
    });
</script>

<style>
  .remove,
  .edit {
    margin-left: 10px;
    cursor: pointer;
  }
</style>

<li class="collection-item listelement">
  <p class="indigo-text text-darken-2 font_links">
    {title} by {author}
    <span class="red-text remove" on:click="{onDelete}">Delete</span>
    <span class="green-text edit" on:click="{onEdit}">Edit</span>
  </p>
</li>
