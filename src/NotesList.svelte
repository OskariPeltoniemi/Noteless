<script>
  // Importataan notet Note-komponentista.
  import Note from './Note.svelte';

  // Exportataan muuttuja ja kaksi funktiota, jota käytetään äitikomponentissa.
  export let notes;
  export let checkDone = () => {};
  export let deleteNote = () => {};

  function poistaNote(id) {
    deleteNote(id);
  }
</script>

<!-- Tulostetaan jokainen olemassa oleva note each:lla. Jos käyttäjä tekee uuden noten, se lisätään
     edellisen noten alle. Jos yhtäkään notea ei ole olemassa, tulostetaan teksti, joka kertoo,
     että noteja ei ole. Note komponentissa olevia funktioita kutsutaan, kun funktio suoritetaan.
     note.id -parametri välitetään funktioille. -->
<div class="notes-list">
  {#if notes.length === 0}
    <p>No notes yet</p>
  {:else}
    {#each notes as note}
      <Note
        note={note}
        on:checkDone={() => checkDone(note.id)}
        onDeleteNote={() => poistaNote(note.id)}
      />
    {/each}
  {/if}
</div>