<script>
  import { authHandlers, authStore } from "../../store/store";
</script>

<p>cool</p>
<button class="logout" on:click={authHandlers.logout}
  ><i class="fa-solid fa-door-open"></i>Log Out</button
>
