<template>
  <div class="detail">
    <!-- Detail View Pokemon -->
    <div class="view no-selectable" v-if="show">
      <!-- Image -->
      <div v-if="pokemon" class="image">
        <div class="img-container">
          <img :src="imageUrl + pokemon.id + '.png'" />
        </div>
      </div>
      <div v-if="pokemon" class="id">
        {{ pokemon.id }}
      </div>
      <!-- Data Info -->
      <div v-if="pokemon" class="data">
        <h2 style="text-transform: capitalize">{{ pokemon.name }}</h2>
        <p>{{ description }}</p>
        <!-- Stats pokemon -->
        <div class="property">
          <div class="left">Experiencia Base</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Peso</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>
        <div class="property">
          <div class="left">Altura</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <div class="left">Velocidad</div>
          <div class="right">{{ pokemon.stats[5].base_stat }}</div>
        </div>
        <!-- Types -->
        <h3>Tipo</h3>
        <div class="types">
          <div
            class="type"
            v-for="(value, index) in types"
            :key="'value' + index"
          >
            {{ value.names[4].name }}
          </div>
        </div>
        <!-- Main stats -->
        <div class="row">
          <div class="column atk">
            ATK<br />{{ pokemon.stats[1].base_stat }}
          </div>
          <div class="column def">
            DEF<br />{{ pokemon.stats[2].base_stat }}
          </div>
        </div>
      </div>
      <!-- Close Button -->
      <h2 v-else>No se encontró el Pokémon</h2>
      <button class="close" @click="closeDetail">Cerrar</button>
    </div>
    <!-- Loading data -->
    <div v-else class="loader" />
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl", "speciesUrl"],
  data: () => {
    return {
      show: false,
      pokemon: {},
      detail: {},
      types: [],
      description: "",
    };
  },
  methods: {
    checkConnection() {
      let req = new Request(this.pokemonUrl);
      fetch(req).then((resp) => {
        if (resp.status === 200) {
          this.fetchData();
        } else {
          this.show = true;
          this.pokemon = null;
        }
      });
    },
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.pokemon = data;
          fetch(this.speciesUrl)
            .then((resp) => {
              if (resp.status === 200) return resp.json();
            })
            .then((data) => {
              this.detail = data;
              this.flavorText(this.detail);
              this.pokemon.types.forEach((value) => {
                fetch(value.type.url)
                  .then((resp) => {
                    if (resp.status === 200) return resp.json();
                  })
                  .then((data) => {
                    this.types.push(data);
                  })
                  .catch((error) => {
                    console.log(error);
                  });
              });
              this.show = true;
            })
            .catch((error) => {
              console.log(error);
            });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    flavorText(detail) {
      for (let index = 0; index <= detail.flavor_text_entries.length; index++) {
        if (detail.flavor_text_entries[index].language.name === "es") {
          this.description = detail.flavor_text_entries[index].flavor_text;
          break;
        }
      }
    },
    closeDetail() {
      this.$emit("closeDetail");
    },
  },
  created() {
    this.checkConnection();
  },
};
</script>

<style>
.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  z-index: 100;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: calc(100% - 20px);
  height: calc(100vh - 20px);
  background: rgba(0, 0, 0, 0.7);
}

.view {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: relative;
  width: 100%;
  max-width: 510px;
  padding: 50px 0 0;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
  -moz-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
  -webkit-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
}

.image {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -60px;
  width: 120px;
  height: 120px;
  background-color: #34495e;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
  -moz-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
  -webkit-box-shadow: 0 15px 30px rgba(0, 0, 0, 0.7);
}

.id {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50px;
  width: 50px;
  height: 25px;
  background-color: white;
  color: #34495e;
  border-radius: 50px;
  overflow: hidden;
  font-weight: bold;
}

.data {
  margin-top: 20px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-bottom: 20px;
}

p {
  text-align: center;
  width: 90%;
}

.property {
  font-size: 14px;
  width: 90%;
  margin: 0 20px 20px;
  text-align: center;
}

.data h3 {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
}

.property {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
  margin-bottom: 10px;
  font-size: 14px;
}

.property .left {
  float: left;
}
.property .right {
  float: right;
}

.types,
.abilities {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 90%;
  max-width: 400px;
  margin-top: 10px;
}

.type {
  margin: 0 10px 10px 0;
  padding: 5px 10px;
  border-radius: 20px;
  color: #fff;
  font-size: 0.8rem;
  letter-spacing: 2px;
  text-transform: capitalize;
  word-wrap: none;
  word-break: keep-all;
  background-color: #0a2e50;
}

.row {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 80%;
  margin-top: 10px;
}

.column {
  display: flex;
  flex-direction: column;
  flex-basis: 100%;
  flex: 1;
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  border-radius: 25px;
  margin: 0 20px 0;
}

.atk {
  border: solid #500a2e;
}
.def {
  border: solid #0a2e50;
}

/* Button Close Detail */
.close {
  cursor: pointer;
  background-color: #41b883;
  border-radius: 20px;
  padding: 10px 20px;
  margin-bottom: 20px;
  color: white;
  outline: none;
  border: none;
  text-align: center;
  font-weight: bold;
  font-size: 12px;
  -webkit-filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.5));
  filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.5));
}
.close:hover {
  background-color: #34495e;
}
/* Loader - Spinner */
.loader {
  border: 16px solid #f3f3f3;
  border-top: 16px solid #34495e;
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>