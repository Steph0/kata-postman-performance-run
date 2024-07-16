<template>
    <div class="notes">
      <i class="notes_icon light-icon-notes"></i>
      <p class="notes_content">
        <slot />
      </p>
    </div>
</template>

<style>
  .notes {
    background-color: rgb(243, 240, 255);
    display: grid;
    justify-content: start;
    padding: 10px;
    margin: 10px;
  }

  .notes_icon {
    grid-column: 1;
    width: 20px;
    /* padding-top: 5px; */
  }

  p.notes_content {
    grid-column: 2;
    margin: 0;
  }
</style>
