<script>
  export let note;
  export let onDeleteNote;
  
  /**Funktiota kutsutaan, kun checkbox valitaan. Noten done-tila päivitetään checkboxin tilan
   * mukaan.
  */
  const onCheckDone = (event) => {
    note.done = event.target.checked;
  };
  
  /**Funktiota kutsutaan, kun delete-nappia painetaan. Se note poistetaan, jonka delete-nappia
   * on painettu. Oikean noten poisto tapahtuu id:n avulla.
  */
  const poistaNote = () => {
    onDeleteNote(note.id);
  }
</script>

<!-- Input, tyyppiä checkbox, on kaksisuuntaisesti sidottu noten done ominaisuuteen.
     Jos checkboxin arvo muuttuu, se välitetään donelle.
     on:change suorittaa siihen kiinnitetyn funktion, kun checkboxia klikataan. -->
<div class="note" class:done={note.done}>
  <input type="checkbox" bind:checked={note.done} on:change={onCheckDone} />
  <p>{note.text}</p>
  <button on:click={poistaNote}>Delete</button>
</div>

<style>
  .note {
    display: flex;
    align-items: center;
    margin-bottom: 0.5rem;
    padding: 1rem;
    border: 2px solid rgb(209, 209, 209);
    border-radius: 0.5rem;
    transition: background-color 0.2s ease;
  }

  .note.done {
    background-color: rgb(240, 240, 240);
  }

  input[type="checkbox"] {
    margin-right: 1rem;
  }

  p {
    flex-grow: 1;
    margin: 0;
    font-size: 1.1rem;
  }

  button {
    padding: 0.5rem 1rem;
    font-size: 1.1rem;
    border: none;
    background-color: #ff3c3c;
    color: white;
  }

  button:hover {
    background-color: #db4141;
  }
</style>