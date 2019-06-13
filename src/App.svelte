<script>
		import { onMount } from 'svelte';

		import AddEditBook from './components/AddEditBook.svelte'
		import Navbar from './components/Navbar.svelte'
		import Books from './components/Books.svelte'
		import { add, getAll, remove, update } from "./utils/operations.js";

	// Application state on the top level
		let books = []

		// 2 Modes for the AddEditComponent: 'edit' and 'add'
		let mode = {
			book: {
				title: "",
      author: "",
      id: ""
			},
			status: 'add',
			btnText: 'Add Book',
			aecmpt: false
		}
		const toggleAdd = e => {
			// console.log(e)
			mode.aecmpt = true;
			mode.status = 'add';
			mode.btnText = 'Add Book';
		}

		const cancel = () => {
			mode.aecmpt = false;
		}

		const addBook = e => {
    const newBook = e.detail;
		// svelte neeeds new assignments for arrays, push doesnt work, wont rerender component
		books = [...books, newBook];
		// add, update, remove books from the local storage
		add(newBook)
		// reset mode to empty book to clear fields in the AddEditComponent
		// status stays on 'add' also after update, see below in update Method
		mode = {
			book: {
				title: "",
      author: "",
      id: ""
			},
			status: 'add',
			btnText: 'Add Book',
			aecmpt: false
		}
	};
	const updateBook = e => {
		// console.log(e)
		books = books.map(el => (el.id === e.detail.id ? e.detail : el));
		update(e.detail)
		mode = {
			book: {
				title: "",
      author: "",
      id: ""
			},
			status: 'add',
			btnText: 'Add Book',
			aecmpt: false
		}
	};
	const removeBook = e => {
		// console.log("e", e)
		books = books.filter(book => book.id !== e.detail);
		remove(e.detail)
	};

	const editBook = e => {
		// console.log(e.detail)
		// console.log("edit book")
		// when clicked on BookItem, editbook is dispatched with e.detail as book param
		// the status is set to 'edit' for the AddEditComponent
		mode = {
			book: e.detail,
			status: 'edit',
			btnText: 'Update Book',
			aecmpt: true // addeditcomponent visible
		}
	};
	// at app start app state books array is filled from local storage
	onMount(()=>{
		books = getAll();
		// console.log("books: ",books)
	})
	</script>
	<div>
			<Navbar on:toggleAdd={toggleAdd} />
			<div class="container">
					{#if mode.aecmpt}
				<AddEditBook mode={mode} on:addBook={addBook} on:updateBook={updateBook} on:cancel={cancel}/>
				{:else}
				<Books books={books} on:removeBook={removeBook} on:editBook={editBook}/>
				{/if}
			</div>
	</div>

