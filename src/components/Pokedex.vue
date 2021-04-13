<template>
  <div class="container">
    <div class="header no-selectable">
      <div class="head">
        <div class="img-container">
          <img id="logo" src="../assets/logo.png" alt="Logo - PokedexVue" />
        </div>
        <h1 class="title">Pokédex</h1>
        <h1 class="title vue">Vue</h1>
      </div>
    </div>
    <ListaPkmn
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"
    />
    <DetallePkmn
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      :speciesUrl="speciesUrl"
      @closeDetail="closeDetail"
    />
    <button id="topBtn" ref="btnTop" @click="topFunction()">
      <span class="arrow up"></span>
    </button>
  </div>
</template>

<script>
import ListaPkmn from "./ListaPkmn.vue";
import DetallePkmn from "./DetallePkmn.vue";
export default {
  components: {
    ListaPkmn,
    DetallePkmn,
  },
  data: () => {
    return {
      imageUrl: "https://pokeres.bastionbot.org/images/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon",
      speciesUrl: "",
      pokemonUrl: "",
      showDetail: false,
    };
  },
  methods: {
    setPokemonUrl(url, sId) {
      this.pokemonUrl = url;
      this.speciesUrl = "https://pokeapi.co/api/v2/pokemon-species/" + sId;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.speciesUrl = "";
      this.showDetail = false;
    },
    topFunction() {
      document.body.scrollTop = 0; // For Safari
      document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera
    },
    scrollFunction() {
      if (
        document.body.scrollTop > 100 ||
        document.documentElement.scrollTop > 100
      ) {
        this.$refs.btnTop.style.display = "block";
      } else {
        this.$refs.btnTop.style.display = "none";
      }
    },
  },
  created() {
    window.addEventListener("scroll", this.scrollFunction);
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-color: #04befe;
  background-image: linear-gradient(to top, #4481eb 0%, #04befe 100%);
  font-size: 1rem;
  font-weight: normal;
}
.header {
  flex-direction: column;
  text-align: center;
}
.head {
  display: flex;
  align-items: center;
  justify-content: center;
}
.title {
  color: #efefef;
  font-family: "Lobster";
  font-size: 3rem;
  -webkit-filter: drop-shadow(5px 5px 5px rgba(51, 51, 51, 0.5));
  filter: drop-shadow(5px 5px 5px rgba(51, 51, 51, 0.5));
}

.title.vue {
  color: #41b883;
  text-shadow: 3px 0 0 #34495e, -3px 0 0 #34495e;
  margin-left: 3px;
}

.img-container {
  position: relative;
}

.img-container:after {
  content: " ";
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 99;
}

#logo {
  width: 60px;
  height: 60px;
  margin-right: 5px;
}

#topBtn {
  display: none;
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 99;
  border: none;
  outline: none;
  cursor: pointer;
  background-color: #34495e;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  -webkit-filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.5));
  filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.5));
}

.arrow {
  border: solid #41b883;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}
.up {
  transform: rotate(-135deg);
  -webkit-transform: rotate(-135deg);
}

#topBtn:hover {
  background-color: #41b883;
}
#topBtn:hover .arrow {
  border: solid #34495e;
  border-width: 0 3px 3px 0;
}
.no-selectable {
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -o-user-select: none;
  user-select: none;
}
</style>