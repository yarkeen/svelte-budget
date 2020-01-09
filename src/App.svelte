

<script>

import {setContext, onMount, beforeUpdate, afterUpdate, onDestroy} from 'svelte';

// components
import Navbar from  "./Navbar.svelte";
import ExpensesList from "./ExpensesList.svelte";
import Totals from "./Totals.svelte";
import ExpenseForm from "./ExpenseForm.svelte";
import Modal from "./Modal.svelte";
// Data

//import expensesData from "./expenses";
// variables
let expenses = [];
let total = 0;
// set editing variables

let setName = "";
let setAmount = null;
let setId = null;

// toggleform variables
let isFormOpen= false;
// reactive

$: total = expenses.reduce((acc, cur)=> { 

    return (acc += cur.amount)    
}, 0)

$: isEditing = setId ? true : false;


// functions

function showForm() {
    isFormOpen=true;
};

function hideForm() {
    isFormOpen=false
    setName="";
    setAmount=null;
    setId=null;
}

function setModifiedExpense (id) {
    let expense = expenses.find(item => item.id === id);
    setId =expense.id;
    setName = expense.name;
    setAmount=expense.amount;
    showForm();
}


function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
    
}

function clearExpenses() {
    expenses=[];
    
};

function addExpense({ name, amount }) {
    let expense = {id:Math.random()* Date.now(), name, amount};
    expenses = [expense,...expenses];
    
};
function editExpense ({name, amount}) {
    expenses = expenses.map( item => {
        return item.id === setId? {...item, name, amount  }: {...item}    

    })
    setId = null;
    setAmount = null;
    setName = "";
    
};

// context
setContext('remove', removeExpense);
setContext('modify', setModifiedExpense);
// local storge
function setLocalStorage(){
    localStorage.setItem("expenses", JSON.stringify(expenses));
};


onMount(() => {
    expenses = localStorage.getItem("expenses")? JSON.parse(localStorage.getItem("expenses"))
    :[];

});

afterUpdate(()=> {
  setLocalStorage()
});

</script>

<Navbar {showForm}/>

<main class="content">
    {#if isFormOpen}

        <Modal>
            <ExpenseForm  
                {addExpense} 
                name={setName} 
                amount={setAmount} 
                {isEditing}
                {editExpense}
                {hideForm}/>
        </Modal>
 {/if}

<Totals title="total expenses"  {total}/>

<ExpensesList {expenses}/>

<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>clear expenses</button>

</main>

