<script>
  import { setClient } from "svelte-apollo";
  import ApolloClient, { gql } from "apollo-boost";
  import { getClient, query } from "svelte-apollo";
  import Book from "./Book.svelte";
  import NavBar from "./_components/NavBar.svelte";
  import Modal from "./_components/Modal.svelte";
  const client = new ApolloClient({
    uri: "http://localhost:1337/graphql"
  });

  const GET_BOOKS = gql`
    query {
      libros {
        id
        Nombre
        valor
        oferta
        autor
        caratula {
          url
          formats
        }
      }
    }
  `;
  let modalData = {};
  let isModalShown = false;
  const books = query(client, { query: GET_BOOKS });
  function showModal(event) {
    client
      .query({
        query: gql`
      {
        libro(id: ${event.detail.data.id}) {
          id
          Nombre
          valor
          oferta
          autor
          editorial
          Categoria
          Tema
          resenia
          idioma
          npaginas
          caratula {
            url
          }
        }
      }
    `
      })
      .then(result => {
        console.log(result);
        isModalShown = true;
        modalData = result.data;
      })
      .catch(err => {
        console.log(err);
      });
  }
  
</script>

<style>
  .container {
    max-width: 960px !important;
  }

  .column {
    height: auto;
  }
</style>

<NavBar />
{#await $books}
  Loading...
{:then result}
  <div class="container">
    <div class="columns is-multiline">
      {#each result.data.libros as libro}
        <div class="column is-one-quarter-desktop ">
          <Book data={libro} on:HandleData={showModal} />
        </div>
      {/each}
    </div>
  </div>
{:catch error}
  Error: {error}
{/await}
{#if isModalShown}
  <Modal {...modalData} on:closeModal={()=>{isModalShown=false}}/>
{/if}
