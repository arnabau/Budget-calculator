<script>
  import { getContext } from 'svelte';

  export let id,
    name = '',
    amount = 0;

  let displayAmount = false;

  const deleteExpense = getContext('delete');
  const setModifyExpense = getContext('modify');
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button
        class="amount-btn"
        on:click={() => {
          displayAmount = !displayAmount;
        }}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4>amount: ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button class="expense-btn edit-btn" on:click={() => setModifyExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => deleteExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
