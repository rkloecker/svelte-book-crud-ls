<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let mode;

  let book = {
    title: mode.book.title,
    author: mode.book.author,
    id: mode.book.id
  };

  let valid = false;
  let error = "";

  const isValid = () => {
    return mode.book.title.trim() && mode.book.author.trim();
  };

  // cancel actions and dispatch to app.svelte component to close the window
  // locally does not work because cloe/open logic was moved to if/else in app.svelte
  const cancel = () => {
    // console.log("cancel");
    mode.book = {
      title: "",
      author: "",
      id: ""
    };
    mode.aecmpt = false;
    dispatch("cancel");
  };

  const onSubmit = e => {
    e.preventDefault();
    if (!isValid()) {
      error = "fill in all fields, please";
      return;
    }
    if (mode.status == "edit") {
      dispatch("updateBook", mode.book);
      // console.log(mode);
    } else if (mode.status == "add") {
      mode.book.id = Date.now().toString();
      dispatch("addBook", mode.book);
    }
    book = {
      title: "",
      author: "",
      id: ""
    };
    error = "";
  };
</script>

<style>
  /* actiff replaces normal active materialize css class to prevent 'label sticking over input-field when prefilled bug'
  for some reason class active doesnt work in svelte, maybe active is some kind of keyword
   */
  .actiff {
    -webkit-transform: translateY(-14px) scale(0.8);
    transform: translateY(-14px) scale(0.8);
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
  }
  h1 {
    font-size: 1.5rem;
    margin-bottom: 3rem;
  }
</style>

<div>
  <br />
  <h1>{mode.btnText}</h1>
  <form on:submit="{onSubmit}">
    <div class="input-field">
      <input type="text" bind:value="{mode.book.title}" />
      <label class="actiff" for="title">Title</label>
    </div>
    <div class="input-field">
      <input type="text" bind:value="{mode.book.author}" />
      <label class="actiff" for="author">Author</label>
    </div>
    <input type="submit" value="{mode.btnText}" class="btn" />
    <input type="button" on:click="{cancel}" value="Cancel" class="btn grey" />
  </form>
  <!-- <h5>{mode.book.title} - {mode.status}</h5> -->
  <h5>{error}</h5>
</div>
