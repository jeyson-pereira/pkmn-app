<template>
  <div class="list">
    <article
      class="no-selectable"
      v-for="(pokemon, index) in pokemons"
      :key="'poke' + index"
      @click="setPokemonUrl(pokemon.url, pokemon.id)"
    >
      <div class="img-container">
        <img :src="imageUrl + pokemon.id + '.png'" />
      </div>
      <h3>{{ pokemon.name }}</h3>
    </article>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],
  data: () => {
    return {
      pokemons: [],
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.apiUrl.concat("?limit=151"));
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function (part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    setPokemonUrl(url, sId) {
      this.$emit("setPokemonUrl", url, sId);
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 90%;
  max-width: 720px;
  padding-bottom: 20px;
}
article {
  height: 150px;
  text-align: center;
  background-color: #fff;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5);
  -moz-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5);
  -webkit-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5);
  padding: 10px;
  text-transform: capitalize;
}
h3 {
  margin: 0;
}
img {
  width: 96px;
  height: 96px;
  -webkit-filter: drop-shadow(5px 5px 5px rgba(0, 0, 0, 0.5));
  filter: drop-shadow(5px 5px 5px rgba(0, 0, 0, 0.5));
}
</style>