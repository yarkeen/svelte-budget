<script>
import {blur, slide, scale, fade, fly} from "svelte/transition";
</script>

<div class="modal-container" in:blur out:fade>
<div class="modal-content"transition:fly="{{y: 150}}">



<slot>

</slot>

</div>


</div>