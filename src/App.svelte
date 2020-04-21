<script>
    import { onMount } from 'svelte'
    import catApi from './cat-api'

	let images = [];
    let favourites = [];

    function addFavourite(image) {
      catApi.addFavourite(image.id).then(() => {
        getFavourites()
      })
    }
    function removeFavourite(favourite) {
      catApi.removeFavourite(favourite.id).then(() => {
        getFavourites()
      })
    }
    function getFavourites() {
      catApi.getFavourites().then(_favourites => {
        favourites = _favourites
      })
    }
    function getImages() {
      catApi.getImages().then(_images => {
        images = _images
      })
    }

    onMount(async () => {
        getImages()
        getFavourites()
    })
</script>

<main>
    <div class="container">
        <h2 class="my-3">Favorit Saya</h2>
        <div class="row">
            {#each favourites as favourite}
            <div class="col-6 col-sm-4 mb-3">
                <button on:click={() => removeFavourite(favourite)}
                        class="btn btn-block btn-danger"
                        >
                        Unlike
                </button>
                <img
                    class="d-block img-fluid"
                    style="max-height: 300px"
                    src={favourite.image.url}
                    alt="a cat"
                />
            </div>
            {/each}
        </div>

        <h2 class="my-3">
            Gallery Kucing
            <button on:click={getImages} class="btn btn-secondary">
                Refresh
            </button>
        </h2>
        <div class="row">
            {#each images as image}
            <div class="col-6 col-sm-4 mb-3">
                <button on:click={() => addFavourite(image)}
                        class="btn btn-block btn-primary"
                        >
                        Like
                </button>
                <img
                    class="d-block img-fluid"
                    style="max-height: 300"
                    src={image.url}
                    alt="a cat"
                />
            </div>
            {/each}
        </div>
    </div>
</main>

<style>
</style>
