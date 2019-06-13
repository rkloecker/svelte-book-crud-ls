<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  let add = false;
  function toggleAdd() {
    add = true;
    dispatch("toggleAdd", add);
  }
</script>
<style>
  .addbook {
    cursor: pointer;
  }
</style>
<div>
  <nav class="light-blue darken-4">
    <div class="container">
      <span class="brand-logo center">
        Books
      </span>
      <ul class="right">
        <li>
          <span class="addbook" on:click="{toggleAdd}">Add Book</span>
        </li>
      </ul>
    </div>
  </nav>
</div>
