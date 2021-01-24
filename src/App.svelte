<script lang="ts">
  import ApolloClient, { gql } from "apollo-boost";
  import { onMount } from "svelte";

const apiURL = "api";
  const projectsQuery = gql`
    query GetProjectsData {
      projects {
        title
        content {
          ... on ComponentSectionsRichText {
            content
          }
          ... on ComponentSectionsImagesSlider {
            images {
              url
            }
          }
          ... on ComponentSectionsLargeMedia {
            media {
              url
            }
          }
        }
      }
    }
  `;

  async function preload({ params, query }) {
    const client = new ApolloClient({
      uri: "http://172.104.238.194:1337/graphql",
      fetch: this.fetch,
    });
    const results = await client.query({
      query: projectsQuery,
      fetchPolicy: "no-cache",
    });
    return { projects: results.data.projects };
  }

//   export let date: date;

//   onMount(async () => {
//     const res = await fetch("/api/date");
//     const newDate = await res.text();
//     date = newDate;
//   });

</script>

<main>
  <h1>Svelte + Node.js API</h1>
  <h2>The date according to Node.js is:</h2>
  <!-- <p>{date ? date : 'Loading date...'}</p> -->

{ await preload then _}
{#each projects as project}
  <h1>{project.title}</h1>
{/each}
{/await}
</main>
