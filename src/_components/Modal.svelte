<script>
  export let libro;
  import { createEventDispatcher } from "svelte";
  let dispatch = createEventDispatcher();
  let cantidad = 1;
  function closeModal(event) {
    dispatch("closeModal");
  }
  let money = new Intl.NumberFormat("es-CL", {
    currency: "CLP",
    style: "currency"
  });
  let monyy = money.format(libro.valor);
  let hasOffer = true ? libro.oferta != 1 : false;
  function clickHandler(){
    if (hasOffer) {
      dispatch('addToCarr',{cantidad:cantidad,valor:libro.valor - (libro.valor * libro.oferta),nombre:libro.Nombre,url:libro.caratula.url});  
    }else{
      dispatch('addToCarr',{cantidad:cantidad,valor:libro.valor,nombre:libro.Nombre,url:libro.caratula.url});
    }
    
  }
</script>

<style>
  .img {
    height: 200px;
    width: 150px;
  }
  .max-text-width {
    max-width: 150px;
  }
  .abitofpadding {
    padding: 1rem;
  }
</style>

<div id="modal" class="modal is-active">
  <div class="modal-background" on:click={closeModal} />
  <div class="modal-content">
    <div class="box">
      <article class="media">
        <div class="media-left">
          <figure class="image is-3by4 img">
            <img src="http://localhost:1337{libro.caratula.url}" alt="Imagen" />
          </figure>
          <br />
          <div class="has-text-centered">
            {#if hasOffer}
              <p class="title">
                <strike>{monyy}</strike>
              </p>
              <p class="title">
                {money.format(libro.valor - libro.valor * libro.oferta)}
              </p>
            {:else}
              <p class="title">{monyy}</p>
            {/if}

            <p>
              <strong>Editorial :</strong>
              {libro.editorial}
            </p>
            {#if libro.tema != null}
              <p>
                <strong>Tema:</strong>
                {libro.tema}
              </p>
            {/if}
            {#if libro.Categoria != null}
              <p class="max-text-width">
                <strong>Categoria:</strong>
                {libro.Categoria}
              </p>
            {/if}
            {#if libro.npaginas != null}
              <p>
                <strong>N° Paginas:</strong>
                {libro.npaginas}
              </p>
            {/if}
            <p>
              <strong>Idioma:</strong>
              {libro.idioma}
            </p>
          </div>
        </div>
        <div class="media-content">
          <div class="content">
            <p>
              <strong>{libro.Nombre}</strong>
              <small>{libro.autor}</small>
              <br />
              {libro.resenia}
            </p>
          </div>

        </div>

      </article>
      <div class="container box has-text-left abitofpadding">
        <div class="control">
          <input min="1" bind:value={cantidad} type="number" style="width:10%"/>
          <button class="button is-success" on:click={clickHandler}>
            <span class="icon is-small">
              <i class="fas fa-plus" />
            </span>
            <span>agregar al carrito</span>
          </button>
        </div>

      </div>
    </div>

  </div>
  <button
    class="modal-close is-large"
    aria-label="close"
    on:click={closeModal} />
</div>
