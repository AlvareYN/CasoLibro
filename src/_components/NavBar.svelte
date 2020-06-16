<script>
  export let carrito;
  function clickHandler() {
    if (isActive) {
      isActive = false;
    } else {
      isActive = true;
    }
  }
  let isActive = false;
  let money = new Intl.NumberFormat("es-CL", {
    currency: "CLP",
    style: "currency"
  });
</script>

<style>
  .text:hover {
    color: rgb(0, 0, 0, 0.5);
    cursor: pointer;
  }

  .newbutton {
    padding: 0;
    border: none;
    background: none;
  }
  .aBitOfPadding {
    padding: 1rem;
  }
  .dropdown-menu {
    width: 500px;
  }
</style>

<div class="container">
  <div class="columns has-text-centered is-vcentered">
    <div class="column">
      <h1 class="title">VentaBook</h1>
    </div>
    <div class="column">
      <p class="control has-icons-left">
        <input class="input" type="text" placeholder="Titulo, categoria" />
        <span class="icon is-left">
          <i class="fas fa-search" aria-hidden="true" />
        </span>
      </p>
    </div>
    <div class="column">
      <div class="dropdown" class:is-active={isActive == true}>
        <div class="dropdown-trigger">
          <button
            on:click={clickHandler}
            class="newbutton text"
            aria-haspopup="true"
            aria-controls="dropdown-menu2">
            {#if carrito.length === 0}
              Carrito
            {:else}Carrito ({carrito.length}){/if}
            <i class="fas fa-shopping-cart" />
          </button>
        </div>
        <div class="dropdown-menu " id="dropdown-menu2" role="menu">
          <div class="dropdown-content aBitOfPadding">
            {#each carrito as productos, i}
              <div class="dropdown-item">

                <article class="media">
                  <figure class="media-left">
                    <p>
                      <strong>#{i + 1}</strong>
                    </p>
                  </figure>
                  <div class="media-content">
                    <div class="content">
                      <strong>{productos.nombre}</strong>
                    </div>
                  </div>
                  <div class="media-right">
                    Cantidad :
                    <strong>{productos.cantidad}</strong>
                    Total :
                    <strong>{money.format(productos.valor * productos.cantidad)}</strong>
                  </div>
                </article>
              </div>

              <hr class="dropdown-divider" />
            {/each}
            <div class="dropdown-item">

              {#if carrito.length > 0}
                <button class="button is-primary">Check Out</button>
              {:else}
                <button class="button is-warning" disabled>Ningun libro</button>
              {/if}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
