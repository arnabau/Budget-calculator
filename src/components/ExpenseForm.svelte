<script>
  // import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';
  import Title from './Title.svelte';

  export let addExpense,
    isEditing,
    editExpense,
    name = '',
    amount = null,
    hideForm;

  $: isEmpty = !name || !amount;

  const handleSubmit = (e) => {
    e.preventDefault();

    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    (name = ''), (amount = null);
  };
</script>

<style>
  /*  */
</style>

<section class="form">
  <Title title="Add Expense" />
  <form class="expense-form" on:submit={handleSubmit}>
    <div class="form-control">
      <label for="name">name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>
    {#if isEmpty}
      <p class="form-empty">Please fill out all form fields</p>
    {/if}
    <button
      disabled={isEmpty}
      type="submit"
      class="btn btn-block"
      class:disabled={isEmpty}>
      {#if isEditing}edit expense{:else}add expense{/if}
    </button>
    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      close
    </button>
  </form>
</section>
