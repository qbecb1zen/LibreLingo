<script lang="typescript" context="module">
  export async function preload(page) {
      const { courseName } = page.params
      const { modules, languageName, repositoryURL } = await import(
          `../../../courses/${courseName}/courseData.json`
      )

      return { courseName, modules, languageName, repositoryURL }
  }
</script>

<script lang="typescript">
  import SkillCard from "../../../components/SkillCard/index.svelte"
  import NavBar from "../../../components/NavBar.svelte"
  import Column from "lluis/Column.svelte"
  import Columns from "lluis/Columns.svelte"
  import Content from "lluis/Content.svelte"
  import Footer from "lluis/Footer.svelte"
  import type { ModulesType } from "../../../types/ModulesType"

  export let courseName = null
  export let modules: ModulesType = null
  export let languageName = null
  export let repositoryURL = null
</script>

<svelte:head>
  <title>LibreLingo - learn {languageName} for free</title>
</svelte:head>

<NavBar hasAuth {repositoryURL} />

{#each modules as { title, skills }}
  <section class="section">
    <div class="container">
      <h2 class="is-size-2">{title}</h2>
      <Columns multiline>
        {#each skills as skill}
          <Column sizeDesktop="1/3" sizeTablet="1/2">
            <SkillCard
              {... { ...skill } }
              practiceHref="{`/course/${courseName}/skill/${skill.practiceHref}`}" />
          </Column>
        {/each}
      </Columns>
    </div>
  </section>
{/each}

<Footer>
  <Content>
    <Columns>
      <Column>
        <strong>LibreLingo</strong>
        by
        <a href="https://github.com/kantord">Dániel Kántor</a>
        and
        <a href="https://github.com/LibreLingo/LibreLingo#contributors">
          various contributors
        </a>
        .
      </Column>
      <Column>
        The source code is licensed
        <a href="https://opensource.org/licenses/AGPL-3.0">AGPL-3.0.</a>
        <br />
        <a href="https://github.com/LibreLingo/LibreLingo">
          Source code available on GitHub.
        </a>
      </Column>
      <Column />
    </Columns>
    <p></p>
  </Content>
</Footer>

<style type="text/scss">
  @import "../../../variables";
  .container {
    padding-right: 20px;
    padding-left: 20px;
  }
</style>
