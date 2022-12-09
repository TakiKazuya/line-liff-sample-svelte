<script>
  import liff from "@line/liff";
  import Reservation from "./routes/reservation/New.svelte"
  import { Alert } from 'sveltestrap';

  async function init() {
    await liff.init({
      liffId: import.meta.env.VITE_LIFF_ID
    });
    if (!liff.isLoggedIn()) {
      liff.login();
    }
  }
</script>

{#await init()}
{:then _}
  <Reservation/>
{:catch e}
  <Alert class="alert-danger">
    LINE認証に失敗しました。
    {e}
  </Alert>
{/await}

<style>
</style>
