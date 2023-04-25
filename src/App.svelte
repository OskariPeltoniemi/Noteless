<script>
	// Importataan onMount-funktio ja tarvittavat komponentit.
	import { onMount } from 'svelte';
	import NotesList from './NotesList.svelte';

	/**Importataan Universal Unique Identifier - luo id:t käyttäjän tekemille noteille.
	 * UUID lähde: https://www.npmjs.com/package/uuid
	 */ 
	import { v4 as uuid } from 'uuid';
	
	let notes = [];
	let inputTeksti = '';
	
	// Funktio käsittelee käyttäjän syöttämän tekstin ja antaa sen inputTeksti-muuttujan arvoksi.
	function kasitteleInput(e) {
	  inputTeksti = e.target.value;
	}
	
	/**Add-painikkeen painaminen suorittaa funktion, joka tarkistaa onko input-kentässä tekstiä.
	 * Jos kenttä sisältää tekstiä, se lisätään notes-arrayyn. Notelle annetaan uniikki id
	 * käyttäen uuid-kirjastoa, tekstiksi asetetaan käyttjän kirjoittama teksti input-kentästä ja
	 * done-booleanin arvoksi false. Jokaisen noten voi sovelluksessa erikseen merkitä tehdyksi.
	*/
	function lisaaNote() {
	  if (inputTeksti.trim() !== '') {
		notes = [...notes, { id: uuid(), text: inputTeksti, done: false }];
		inputTeksti = '';
	  }
	}
	
	/**Funktio merkitsee noten suoritetuksi, kun sen checkboxia klikataan. Oikea note löydetään
	 * id:n avulla.
	*/
	function valmisNote(id) {
	  notes = notes.map(note => {
		if (note.id === id) {
		  note.done = !note.done;
		}
		return note;
	  });
	}
	
	// Funktio poistaa oikean noten id:n avulla, kun delete-painiketta klikataan.
	function poistaNote(id) {
	  notes = notes.filter(note => note.id !== id);
	}
	
	// Hakee tallennetut notet localStoragesta, jos se sisältää niitä.
	onMount(() => {
	  const tallennetut = localStorage.getItem('notes');
	  if (tallennetut) {
		notes = JSON.parse(tallennetut);
	  }
	});
	
	/**localStorage sisältää kaikki käyttäjän luomat notet. LocalStorage on window-objektin
	 * ominaisuus, joka tallentaa datan käyttäjän selaimeen.
	 * Notes-array muutetaan JSON stringiksi. Tämä mahdollistaa niiden helpon käsittelyn.
	 * LocalStoragen lähde: https://dev.to/danawoodman/svelte-quick-tip-connect-a-store-to-local-storage-4idi
	*/ 
	$: localStorage.setItem('notes', JSON.stringify(notes));
  </script>
	
<main>
	<h1>Noteless</h1>
	
	<div class="add-note">
	  <input type="text" bind:value={inputTeksti} on:input={kasitteleInput} placeholder="Add a note..." />
	  <button on:click={lisaaNote}>Add</button>
	</div>
	
	<NotesList
	  notes={notes}
	  checkDone={valmisNote}
	  deleteNote={poistaNote}
	/>
  </main>

<style>
main {
    max-width: 600px;
    margin: 0 auto;
    padding: 1rem;
  }

  h1 {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  .add-note {
    display: flex;
    margin-bottom: 1rem;
  }

  input[type="text"] {
    flex-grow: 1;
    margin-right: 1rem;
    padding: 0.5rem;
    font-size: 1.1rem;
    border: none;
    border-bottom: 2px solid rgb(202, 202, 202);
  }

  input[type="text"]:focus {
    outline: none;
    border-bottom-color: rgb(104, 104, 104);
  }

  button {
    padding: 0.5rem 1rem;
    font-size: 1.1rem;
    border: none;
    background-color: rgb(51, 51, 51);
    color: white;
  }

  button:hover {
    background-color: rgb(100, 100, 100);
  }
</style>