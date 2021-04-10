<template>
  <div class="container">
    <div class="header no-selectable">
      <div class="img-container">
        <img id="logo" src="../assets/logo.png" alt="Logo - PokedexVue" />
      </div>
      <h1 class="title">Pokédex</h1>
      <h1 class="title vue">Vue</h1>
    </div>
    <ListaPkmn :imageUrl="imageUrl" :apiUrl="apiUrl" />
  </div>
</template>

<script>
import ListaPkmn from "./ListaPkmn.vue";
export default {
  components: {
    ListaPkmn,
  },
  data: () => {
    return {
      imageUrl: "https://pokeres.bastionbot.org/images/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
    };
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 0;
  padding: 0;
  background-color: #04befe;
  background-image: linear-gradient(to top, #4481eb 0%, #04befe 100%);
  font-size: 1rem;
  font-weight: normal;
}
.header {
  display: flex;
  align-items: center;
  justify-content: space-around;
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

.no-selectable {
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -o-user-select: none;
  user-select: none;
}
</style>