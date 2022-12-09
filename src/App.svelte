<script>
  import liff from "@line/liff";
  import Reservation from "./routes/reservation/New.svelte"
  import { Alert } from 'sveltestrap';

  console.log('VITE_LIFF_ID:', import.meta.env.VITE_LIFF_ID)
  async function init() {
    await liff.init({
      liffId: '1657708125-0aOKbALj'
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
