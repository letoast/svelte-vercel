<script>
  import ApolloClient, { gql } from "apollo-boost";
  import { fetch } from "cross-fetch";
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

  let projects = [];

  const client = new ApolloClient({
    uri: "http://172.104.238.194:1337/graphql",
    fetch: fetch,
  });

  onMount(async () => {
    let results = await client.query({
      query: projectsQuery,
      fetchPolicy: "no-cache",
    });
    projects = results.data.projects;
  });
</script>

<main>
  <h1>Svelte + Node.js API</h1>
  <h2>The date according to Node.js is:</h2>
  <!-- <p>{date ? date : 'Loading date...'}</p> -->

  {#each projects as project}
        <h1>{project.title}</h1>
    {/each}
</main>
