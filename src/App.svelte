<script>
  import Sidebar from './components/Sidebar.svelte';
  import Content from './components/Content.svelte';

  export let config;

  let envId = 0;
  $: env = config.environments[envId];

  const jsonUrl = window.location.origin + window.INSOMNIA_URL;
  const runInInsomniaLink = `https://insomnia.rest/run/?label=${encodeURIComponent(config.workspace.name)}&uri=${encodeURIComponent(jsonUrl)}`;

  let menuVisible = false;

  function toggleHamburger() {
    menuVisible = !menuVisible;
  }

  const logoUrl = process.env.NODE_ENV === 'demo'
    ? '/insomnia-documenter/logo.png'
    : '/logo.png';
</script>

<svelte:head>
  <title>{config.workspace.name}</title>
</svelte:head>

<header>
  <div class="header-left">
    <a href="javascript:;" class="hamburger-toggler" on:click='{toggleHamburger}'>
      <i class="fa fa-bars"></i>
    </a>

    <div class="logo">
      <img src={logoUrl} alt={config.workspace.name} />
    </div>

    <h1 class="title">{config.workspace.name}</h1>
  </div>
  <div class="header-right">
    <div class="run">
      <a href={runInInsomniaLink} target="_blank">
        <img src="https://insomnia.rest/images/run.svg" alt="Run in Insomnia" />
      </a>
    </div>
    <div class="environment">
      <span>Environment:</span>
      <select bind:value={envId}>
        {#each config.environments as environment, idx}
          <option value={idx}>{environment.name}</option>
        {/each}
      </select>
    </div>
  </div>
</header>

<section class="wrapper">
  <Sidebar config={config} visible={menuVisible} />
  <Content
    requests={config.requests}
    groups={config.groups}
    workspace={config.workspace}
    cookiejars={config.cookiejars}
    {env}
  />
</section>

<style type="scss" global>
  @import './styles/main';

  header {
    box-sizing: border-box;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    border-bottom: 1px solid #dedede;
    background: #fff;
    z-index: 10000;
    display: flex;
    justify-content: space-between;
    height: 60px;
    overflow: hidden;
  }

  header .header-left, header .header-right {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
  }

  header .title {
    padding: 0 10px;
    margin: 0;
    font-size: 22px;
    font-weight: 600;
    display: inline-block;
    vertical-align: middle;
  }

  header .hamburger-toggler {
    vertical-align: middle;
    font-size: 22px;
    color: #000;
  }

  header .logo {
    display: inline-block;
    vertical-align: middle;
    padding: 0 5px;
    margin-left: 30px;
    width: 48px;
    height: 48px;
  }

  header .logo img {
    width: 100%;
    height: 100%;
  }

  header .environment {
    font-size: 13px;
    padding: 0 30px;
    display: inline-block;
    vertical-align: middle;
  }

  header .environment select {
    margin-bottom: 0;
  }

  header .run {
    display: inline-block;
    vertical-align: middle;
  }

  .wrapper {
    margin-top: 60px;
  }
</style>
