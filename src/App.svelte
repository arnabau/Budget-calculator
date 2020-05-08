<script>
  import { setContext } from 'svelte';
  import Navbar from './components/Navbar.svelte';
  import ExpensesList from './components/ExpensesList.svelte';
  import ExpenseData from './expenses';
  import Totals from './components/Totals.svelte';
  import ExpenseForm from './components/ExpenseForm.svelte';

  let expenses = [...ExpenseData],
    setName = '',
    setAmount = null,
    setId = null,
    isFormOpen = false;

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  const deleteExpense = (id) => {
    expenses = expenses.filter((item) => item.id !== id);
  };

  const clearExpenses = () => {
    expenses = [];
  };

  const addExpense = ({ name, amount }) => {
    let expense = { id: Math.round(Math.random() * Date.now()), name, amount };
    expenses = [expense, ...expenses];
  };

  const setModifiedExpense = (id) => {
    let expense = expenses.find((item) => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  };

  const editExpense = ({ name, amount }) => {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });

    (setId = null), (setAmount = null), (setName = '');
  };

  const showForm = () => {
    isFormOpen = true;
  };

  const hideForm = () => {
    isFormOpen = false;
    (setName = ''), (setAmount = null), (setId = null);
  };

  setContext('delete', deleteExpense);
  setContext('modify', setModifiedExpense);
</script>

<style>

</style>

<Navbar {showForm} />

<main class="content">
  {#if isFormOpen}
    <ExpenseForm
      {addExpense}
      name={setName}
      amount={setAmount}
      {isEditing}
      {editExpense}
      {hideForm} />
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
