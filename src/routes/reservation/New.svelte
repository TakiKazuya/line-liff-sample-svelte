<script>
  import liff from "@line/liff";
  import { Router, Link, Route } from "svelte-routing";
  import {Label, Input, Button, Container, Row, Col} from 'sveltestrap'

  let displayName = "";
  let accessToken = liff.getAccessToken();
  let reserved_at = ""

  async function fetchProfile() {
    const profile = await liff.getProfile()
    displayName = profile.displayName;
  }

  export const basepath = import.meta.env.BASE_PATH
  export const url = "";
</script>

<Router basepath="{basepath}" url="{url}">
  <main>
    <Container>
      <Route path="/">
        <Row>
          <Col>
            <h1>予約フォーム</h1>
          </Col>
        </Row>
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
    </Container>
  </main>
</Router>
