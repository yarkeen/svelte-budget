<script>
import {blur, slide, scale, fade, fly} from "svelte/transition";
import {flip} from "svelte/animate";
import SectionTitle from './Title.svelte';
import Expense from './Expense.svelte';
export let expenses = [];


</script>


<section>
<SectionTitle title="expense list" />
<ul>
{#each expenses as expense, index (expense.id)}
   <div in:fly= "{{x: 200, delay: index*100}}" out:fly ="{{x: -200}}" anmate:flip>
   <Expense {...expense}  /> 
   </div>
   {:else}
   <h2> No expenses added to the list</h2>
{/each}
</ul>
</section>