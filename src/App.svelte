<script>
  const streamers = [
    "ESL_SC2",
    "OgamingSC2",
    "cretetion",
    "freecodecamp",
    "storbeck",
    "habathcx",
    "RobotCaleb",
    "noobs2ninjas"
  ];
  let fetchedData = {
    batchcomplete: "",
    query: { searchinfo: { totalhits: 0 }, search: [] }
  };

  let inputText = "";

  async function send() {
    const response = await fetch(
      // `https://api.twitch.tv/helix/search/channels?query=a_seagull`,
      // {
      //   method: "GET",
      //   headers: {
      //     client_id: "xiumu0315cxptogb85ow6jsvbn07nk",
      //     Authorization: "Bearer 2gbdx6oar67tqtcmt49t3wpcgycthx"
      //   }
      // }
      "https://id.twitch.tv/oauth2/authorize?client_id=xiumu0315cxptogb85ow6jsvbn07nk&redirect_uri=http://localhost&response_type=token&scope=viewing_activity_read",
      {
        method: "GET"
        // headers: {
        //   //   client_id: "xiumu0315cxptogb85ow6jsvbn07nk"
        //   accept: "application/vnd.twitchtv.v5+json"
        // }
      }
    );
    fetchedData = await response.json();
    console.log(fetchedData);
  }

  const handleSearch = e => {
    if (e.key === "Enter") {
      send();
    }
  };
</script>

<style>
  .main {
    margin: 2em auto auto;
    width: 450px;
    min-height: 100vmax;
  }
  .searchbar {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
  .group-left {
    display: flex;
    flex-direction: row;
  }
  .searchbar button,
  .searchbar input {
    height: 36px;
    border-radius: 0;
    border-color: lightslategrey;
  }
  .searchbar button {
    background-color: lightyellow;
  }
  .searchbar__go button {
    border-left: 0px;
  }
  .listview__item {
    border-bottom: 1px solid lightslategrey;
  }
  .listview__item a {
    color: unset;
  }
  .listview__item a:hover {
    text-decoration: unset;
  }
</style>

<!-- https://dev.twitch.tv/docs/authentication/getting-tokens-oauth -->
<!-- Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://www.twitch.tv/login?client_id=xiumu0315cxptogb85ow6jsvbn07nk&redirect_params=client_id%3Dxiumu0315cxptogb85ow6jsvbn07nk%26origin%3D%252A%26redirect_uri%3Dhttp%253A%252F%252Flocalhost%253A5000%26response_type%3Dtoken%26scope%3Dviewing_activity_read. 
(Reason: CORS header ‘Access-Control-Allow-Origin’ missing). -->

<div class="main">
  <div class="searchbar">
    <div class="group-left">
      <div class="searchbar__field">
        <input type="text" bind:value={inputText} on:keydown={handleSearch} />
      </div>
      <div class="searchbar__go">
        <button on:click={send}>Go</button>
      </div>
    </div>
    <div class="searchbar__random">
      <a href="https://en.wikipedia.org/wiki/Special:Random" target="_blank">
        <button>I'm Feeling Random</button>
      </a>
    </div>
  </div>

  <div class="listview">
    {#if fetchedData.length === 0}
      <div>Search Wikipedia</div>
    {:else}
      {fetchedData}
      <!-- {#each fetchedData.query.search as item}
        <div class="listview__item">
          <a
            href={`https://en.wikipedia.org/?curid=${item.pageid}`}
            target="_blank">
            <h3>{item.title}</h3>
            <p>
              {@html item.snippet}
            </p>
          </a>
        </div>
      {/each} -->
    {/if}
  </div>
</div>
