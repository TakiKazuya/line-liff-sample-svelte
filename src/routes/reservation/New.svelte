<script>
  import liff from "@line/liff";
  import { Router, Link, Route } from "svelte-routing";
  import {Label, Input, Button} from 'sveltestrap'

  let displayName = "";
  let accessToken = liff.getAccessToken();
  let reserved_at = ""

  async function fetchProfile() {
    const profile = await liff.getProfile()
    console.log('profile', profile)
    displayName = profile.displayName;
  }

  export let url = import.meta.env.BASE_URL;
</script>

<Router url="{url}">
  <main>
    <div class="mx-auto md:w-2/3 w-full mt-2">
      <Route path="/">
        <h1>予約フォーム</h1>
        {#await fetchProfile()}
        {:then profile}
          <div class='mb-2 mx-2'>
            <p>名前：{displayName}</p>
          </div>
          <div class='mb-2 mx-2'>
            <Label for='reservation-date-input' class='block mb-2'>予約日時</Label>
            <Input id='reservation-date-input' type="datetime-local" bind:value="{reserved_at}"/>
          </div>
          <div class='mb-2 mx-2'>
            <Link to="complete">
              <Button block>送信</Button>
            </Link>
          </div>
        {/await}
        <input type="hidden" value="{accessToken}">
      </Route>
      <Route path="complete">
        <h1>予約完了</h1>
        <p>名前：{displayName}</p>
        <p>日時：{reserved_at}</p>
      </Route>
    </div>
  </main>
</Router>
