<script>
  import { createEventDispatcher } from "svelte";
  let dispatch = createEventDispatcher();
  export let data;
  let money = new Intl.NumberFormat("es-CL", {
    currency: "CLP",
    style: "currency"
  });
  let monyy = money.format(data.valor);
  function handleData(event){
    dispatch("HandleData",{data});
  }
</script>

<style>
  .card {
    height: 100%;
  }

  .same-size {
    max-height: 300px;
  }
  .valor {
    position: absolute;
    bottom: 0;
    padding-bottom: 1rem;
    padding-top: 1rem;
  }
</style>

<div class="card" on:click={handleData}>
  <div class="card-image">
    <figure class="image is-2by5">
      <img
        class="same-size"
        src="http://localhost:1337{data.caratula.url}"
        alt="Placeholder" />
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
      <div class="media-content">
        <p class="title is-4">{data.Nombre}</p>
        <p class="subtitle is-6">{data.autor}</p>
      </div>
    </div>

    <div class="media valor has-text-centered">
      <div class="media-content">
        <p class="is-size-5 ">{monyy}</p>
      </div>
    </div>
  </div>
</div>
