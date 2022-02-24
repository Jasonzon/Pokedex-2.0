<template>
  <BannerComponent class="banner"/>
  <SelectComponent v-model:modelValue="searchBar" v-model:idValue="idsort" v-bind:idValue="idsort" v-model:reverseValue="reverseValue" v-bind:reverseValue="reverseValue" class="select"/>
  <div v-show="this.displayed" class="pokemon-parent">
  <img v-if="pokemonInfo.id!==1" class="left-arrow" alt="left-arrow" :src="left" width="45" height="45" v-on:click="scrollToTop((pokemonInfo.id-1).toString())"/>
  <img v-else class="left-arrow-disabled" alt="left-arrow" :src="left" width="45" height="45"/>
    <div class="pokemon" v-show="loadedPokemon===true">
      <div class="flex-pokemon">
      <img class="pkmn-img" @load="loadedPokemon=true" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + `${pokemonInfo.id}` + '.svg'" width="350" height="350"/>
      <div class="principal">
      <div v-if="pokemonInfo.evolves_from_species!==undefined && pokemonInfo.evolves_from_species!==null && pokemonList.find(element => element.name === pokemonInfo.evolves_from_species.name)!==undefined">
      <span>evolve from:</span>
      <div v-if="pokemonInfo.evolves_from_species!==undefined && pokemonInfo.evolves_from_species!==null">
        <img v-if="pokemonList.find(element => element.name === pokemonInfo.evolves_from_species.name)!==undefined" v-on:click="scrollToTop(pokemonInfo.evolves_from_species.name)" class="pre-evolution" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + `${pokemonInfo.evolves_from_species.url.match(/\d/g).join('').substring(1)}` + '.svg'" width="100" height="100" />
      </div>
      </div>
      <div v-if="evolutionChain.chain!==undefined && ((evolutionChain.chain.species.name===pokemonInfo.name && evolutionChain.chain.evolves_to.length!==0 && pokemonList.find(element => element.name === evolutionChain.chain.evolves_to[0].species.name)!==undefined) || (evolutionChain.chain.evolves_to.length!==0 && pokemonInfo.name===evolutionChain.chain.evolves_to[0].species.name && evolutionChain.chain.evolves_to[0].evolves_to.length!==0 && pokemonList.find(element => element.name === evolutionChain.chain.evolves_to[0].evolves_to[0].species.name)!==undefined))">
      <span>evolves to:</span>
      <template v-if="evolutionChain.chain!==undefined && pokemonInfo.name===evolutionChain.chain.species.name">
        <div v-for="pokemon in evolutionChain.chain.evolves_to" :key="pokemon.species.name">
          <img v-if="pokemonList.find(element => element.name === pokemon.species.name)!==undefined" v-on:click="scrollToTop(pokemon.species.name)" class="evolves" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + `${pokemon.species.url.match(/\d/g).join('').substring(1)}` + '.svg'" width="100" height="100" />
        </div>
      </template>
      <template v-if="evolutionChain.chain!==undefined && evolutionChain.chain.evolves_to.length!==0 && pokemonInfo.name!==evolutionChain.chain.species.name && pokemonInfo.name===evolutionChain.chain.evolves_to[0].species.name && evolutionChain.chain.evolves_to[0].evolves_to.length!==0">
        <div v-for="pokemon in evolutionChain.chain.evolves_to[0].evolves_to" :key="pokemon.species.name">
          <img v-if="pokemonList.find(element => element.name === pokemon.species.name)!==undefined" v-on:click="scrollToTop(pokemon.species.name)" class="evolves" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + `${pokemon.species.url.match(/\d/g).join('').substring(1)}` + '.svg'" width="100" height="100" />
        </div>
      </template>
      </div>
      <div>
      <div>
        <span>shiny:</span>
        <div>
        <img class="shiny" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/shiny/'+ `${pokemonInfo.id}` +'.png'" width="100" height="100" />
        </div>
      </div>
      <div v-if="pokemonInfo.varieties!==undefined && pokemonInfo.varieties.find(element => element.pokemon.name.includes('mega'))!==undefined">
      <span>mega:</span>
      <template v-for="pokemon in pokemonInfo.varieties" :key="pokemon.name">
        <div v-if="pokemon.pokemon.name.includes('mega') && pokemonInfo.name!=='charizard' && pokemonInfo.name!=='mewtwo'">
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.id.toString().length===1" class="mega" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_00'+ `${pokemonInfo.id}` +'-mega.png'" width="100" height="100" />
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.id.toString().length===2" class="mega" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_0'+ `${pokemonInfo.id}` +'-mega.png'" width="100" height="100" />
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.id.toString().length===3" class="mega" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_'+ `${pokemonInfo.id}` +'-mega.png'" width="100" height="100" />
        </div>
      </template>
        <div v-if="pokemonInfo.id!==undefined && pokemonInfo.name==='charizard' || pokemonInfo.name==='mewtwo'" class="mega">
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.name==='charizard'" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_00'+ `${pokemonInfo.id}` +'-mega-x.png'" width="100" height="100" />
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.name==='charizard'" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_00'+ `${pokemonInfo.id}` +'-mega-y.png'" width="100" height="100" />
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.name==='mewtwo'" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_'+ `${pokemonInfo.id}` +'-mega-x.png'" width="100" height="100" />
        <img v-if="pokemonInfo.id!==undefined && pokemonInfo.name==='mewtwo'" :src="'https://www.pokencyclopedia.info/sprites/artworks/art_global-link/art_glolnk_'+ `${pokemonInfo.id}` +'-mega-y.png'" width="100" height="100" />
        </div>
      </div>
      <div v-if="pokemonInfo.varieties!==undefined && pokemonInfo.varieties.length>1 && (pokemonInfo.varieties.find(element => element.pokemon.name.includes('alola'))!==undefined || pokemonInfo.varieties.find(element => element.pokemon.name.includes('galar'))!==undefined)">
      <span>form:</span>
        <div v-for="(pokemon,index) in pokemonInfo.varieties" :key="index">
          <img v-if="pokemon.pokemon.name.includes('alola') || pokemon.pokemon.name.includes('galar')" class="form" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/' + `${pokemon.pokemon.url.match(/\d/g).join('').substring(1)}` + '.png'" width="100" height="100" />
        </div>
      </div>
      </div>
        <div>
        <div class="name-id">
          <span v-if="pokemonInfo.name !== undefined" class="name">{{pokemonInfo.name.charAt(0).toUpperCase() + pokemonInfo.name.slice(1)}}</span>
          <span class="id">#{{pokemonInfo.id}}</span>
        </div>
        <div class="types">
          <span v-if="pokemon2Info.types!==undefined && pokemon2Info.types.length===1" :class="'pokemon-type-' + `${pokemon2Info.types[0].type.name}`">{{pokemon2Info.types[0].type.name}}</span>
          <span v-if="pokemon2Info.types!==undefined && pokemon2Info.types.length===2" :class="'pokemon-type-' + `${pokemon2Info.types[0].type.name}`">{{pokemon2Info.types[0].type.name}}</span>
          <span v-if="pokemon2Info.types!==undefined && pokemon2Info.types.length===2" :class="'pokemon-type-' + `${pokemon2Info.types[1].type.name}`">{{pokemon2Info.types[1].type.name}}</span>
        </div>
        </div>
      </div>
      <div class="chara">
        <span v-if="pokemonInfo.flavor_text_entries!==undefined" class="flavor">{{pokemonInfo.flavor_text_entries.find(element => element.language.name==='en').flavor_text}}</span>
        <span v-if="pokemonInfo.genera!==undefined" class="genera">{{pokemonInfo.genera[7].genus}}</span>
        <span v-if="pokemon2Info.height!==undefined" class="height">{{parseFloat(pokemon2Info.height)/10}} m</span>
        <span v-if="pokemon2Info.weight!==undefined" class="weight">{{parseFloat(pokemon2Info.weight)/10}} kg</span>
        <span v-if="pokemonInfo.color!==undefined" class="color">color: {{pokemonInfo.color.name}}</span>
        <span v-if="pokemonInfo.habitat!==undefined && pokemonInfo.habitat!==null" class="habitat">habitat: {{pokemonInfo.habitat.name}}</span>
        <span v-if="pokemonInfo.shape!==undefined" class="shape">shape: {{pokemonInfo.shape.name}}</span>
        <div class="stats">
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[0].stat.name}}: {{pokemon2Info.stats[0].base_stat}}</span>
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[1].stat.name}}: {{pokemon2Info.stats[1].base_stat}}</span>
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[2].stat.name}}: {{pokemon2Info.stats[2].base_stat}}</span>
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[3].stat.name}}: {{pokemon2Info.stats[3].base_stat}}</span>
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[4].stat.name}}: {{pokemon2Info.stats[4].base_stat}}</span>
          <span v-if="pokemon2Info.stats!==undefined">{{pokemon2Info.stats[5].stat.name}}: {{pokemon2Info.stats[5].base_stat}}</span>
        </div>
      </div>
      </div>
      <img class="cross" alt="cross" :src="cross" width="20" height="20" @click="displayed=false"/>
      <div v-if="loadedPokemon===false" class="poke"><div class="ball"></div></div>
    </div>
    <img v-if="pokemonInfo.id!==151" :src="right" alt="right-arrow" class="right-arrow" width="45" height="45" v-on:click="scrollToTop((pokemonInfo.id+1).toString())"/>
    <img v-else :src="right" alt="right-arrow" class="right-arrow-disabled" width="45" height="45"/>
  </div>
  <div class="pokemon-list">
  <div v-if="this.imgLoaded!==151 && !loadedFirst" class="poke">
    <div class="ball"></div>
  </div>
    <ul class="grid" v-show="this.imgLoaded==151 || loadedFirst">
    <template v-if="idsort==='true' && reverseValue==='true'">
    <template v-for="(Pokemon,index) in pokemonList" :key="Pokemon.name">
      <li v-show="(index <= nbPoke || searchBar!=='') && (Pokemon.name.toUpperCase().includes(searchBar.toUpperCase()) || Pokemon.url.match(/\d/g).join('').substring(1).includes(this.searchBar))">
        <section v-on:click="scrollToTop(Pokemon.name)">
          <PokemonComponent @loaded="increase" v-bind:name="Pokemon.name" v-bind:url="Pokemon.url" v-bind:id="Pokemon.url.match(/\d/g).join('').substring(1)" v-model:loaded="imgLoaded" />
        </section>
      </li>
    </template>
    </template>
    <template v-if="idsort==='true' && reverseValue==='false'">
    <template v-for="(Pokemon,index) in pokemonList.slice().reverse()" :key="Pokemon.name">
      <li v-show="(index <= nbPoke || searchBar!=='') && (Pokemon.name.toUpperCase().includes(searchBar.toUpperCase()) || Pokemon.url.match(/\d/g).join('').substring(1).includes(this.searchBar))">
        <section v-on:click="scrollToTop(Pokemon.name)">
          <PokemonComponent @loaded="increase" v-bind:name="Pokemon.name" v-bind:url="Pokemon.url" v-bind:id="Pokemon.url.match(/\d/g).join('').substring(1)" v-model:loaded="imgLoaded" />
        </section>
      </li>
    </template>
    </template>
    <template v-if="idsort==='false' && reverseValue==='true'">
    <template v-for="(Pokemon,index) in pokemonList.slice(0).sort((a,b) => a.name > b.name ? 1 : -1)" :key="Pokemon.name">
      <li v-show="(index <= nbPoke || searchBar!=='') && (Pokemon.name.toUpperCase().includes(searchBar.toUpperCase()) || Pokemon.url.match(/\d/g).join('').substring(1).includes(this.searchBar))">
        <section v-on:click="scrollToTop(Pokemon.name)">
          <PokemonComponent @loaded="increase" v-bind:name="Pokemon.name" v-bind:url="Pokemon.url" v-bind:id="Pokemon.url.match(/\d/g).join('').substring(1)" v-model:loaded="imgLoaded" />
        </section>
      </li>
    </template>
    </template>
    <template v-if="idsort==='false' && reverseValue==='false'">
    <template v-for="(Pokemon,index) in pokemonList.slice(0).sort((a,b) => a.name > b.name ? 1 : -1).slice().reverse()" :key="Pokemon.name">
      <li v-show="(index <= nbPoke || searchBar!=='') && (Pokemon.name.toUpperCase().includes(searchBar.toUpperCase()) || Pokemon.url.match(/\d/g).join('').substring(1).includes(this.searchBar))">
        <section v-on:click="scrollToTop(Pokemon.name)">
          <PokemonComponent @loaded="increase" v-bind:name="Pokemon.name" v-bind:url="Pokemon.url" v-bind:id="Pokemon.url.match(/\d/g).join('').substring(1)" v-model:loaded="imgLoaded" />
        </section>
      </li>
    </template>
    </template>
    </ul>
  </div>
  <section v-if="this.imgLoaded===151 || loadedFirst">
    <FooterComponent />
  </section>
</template>

<script>
import SelectComponent from './components/Select.vue'
import BannerComponent from "./components/Banner.vue"
import FooterComponent from "./components/Footer.vue"
import PokemonComponent from "./components/Pokemon.vue"
import cross from "./assets/cross.png"
import left from "./assets/arrow_left.png"
import right from "./assets/arrow_right.png"

export default {
  name: 'App',
  components: {
    SelectComponent,
    BannerComponent,
    FooterComponent,
    PokemonComponent
  },
  data: function() {
    return {
      pokemonList: [],
      searchBar: "",
      imgLoaded: 0,
      loadedFirst: false,
      displayed: false,
      pokemonInfo: {},
      loadedPokemon: false,
      cross: cross,
      pokemon2Info: {},
      idsort: "true",
      nbPoke: 151,
      right: right,
      left: left,
      evolutionChain: {},
      reverseValue: "true"
    }
  },
  methods: {
    getPokemons: async function() {
        const pokemon = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151")
        const pokemonJson = await pokemon.json()
        const pokemonResults = await pokemonJson.results
        this.pokemonList = pokemonResults
    },
    increase: function() {
      this.imgLoaded++
      if (this.imgLoaded === 151) {
        this.loadedFirst = true
      }
    },
    scrollToTop: function(name) {
      this.displayed = true
      this.getPokemon(name)
      this.getPokemon2(name)
      window.scrollTo(0, 220)
    },
    getEvolutions: async function(pkmn) {
      const pokemon_chain = await fetch(pkmn.evolution_chain.url)
      const chainJson = await pokemon_chain.json()
      this.evolutionChain = chainJson
    },
    getPokemon: async function(name) {
        const pokemon_specie = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${name}`)
        const pokemon_specieJson = await pokemon_specie.json()
        this.pokemonInfo = pokemon_specieJson
        this.getEvolutions(pokemon_specieJson)
    },
    getPokemon2: async function(name) {
      const pokemon2 = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`)
      const pokemon2Json = await pokemon2.json()
      this.pokemon2Info = pokemon2Json
    }
  }, 
  beforeMount() {
    this.getPokemons()
  }
}
</script>

<style>
.principal {
  display:flex;
  flex-direction:column;
  justify-content:space-between;
}

.mega, .pre-evolution, .evolves, .form, .shiny {
  border-radius:5px 5px 5px 5px;
  border:solid black 1px;
  background-color:#F5F5F5;
  padding:3%;
}

.pre-evolution {
  cursor:pointer;
}

.evolves {
  cursor:pointer;
}

.pre-evolution:hover {
  background-color:#E0E0E0;
}

.evolves:hover {
  background-color:#E0E0E0;
}

.pkmn-img {
  margin-right:1%;
}

.pokemon-parent {
  display:flex;
  flex-direction:row;
  justify-content:space-between;
}

.left-arrow, .right-arrow {
  background-color:white;
  border-radius:5px 5px 5px 5px;
  border:solid black 2px;
  height:100%;
  padding:1%;
  cursor:pointer;
  margin:2%;
}

.left-arrow:hover, .right-arrow:hover {
    background-color:#E8E8E8;
}

.left-arrow-disabled, .right-arrow-disabled {
  background-color:grey;
  border-radius:5px 5px 5px 5px;
  border:solid black 2px;
  height:100%;
  padding:1%;
  margin:2%;
}


.types {
  display:flex;
  flex-direction:row;
  justify-content:space-around;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width:100%;
  margin-bottom:1%;
}

body {
  width:100%;
  height:100%;
  background-color:#404040;
  margin:0;
  overflow-x:hidden;
}

ul {
  list-style-type: none;
  margin-right:4%;
}

.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}

@font-face {
  font-family: "Minecraft";
  src: local("Minecraft"), url(./fonts/Minecraft.ttf) format("truetype");
}

p {
  color:white;
}

.poke {
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
  width:100%;
  margin-top:5%;
}

.ball {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: white;
  position: relative;
  box-shadow: -20px 0 rgba(0, 0, 0, 0.1) inset;
  animation: roll 1s ease-in-out infinite;
  background: linear-gradient(
    to bottom,
    #e83e35 0%,
    #e83e35 50.5%,
    #ffffff 50.51%,
    #ffffff 100%
  );
}

.ball:after {
  content: "";
  position: absolute;
  top: calc(100px - 3px);
  left: 0;
  width: 200px;
  height: 6px;
  background: #3f3f3f;
}

.ball:before {
  content: "";
  position: absolute;
  top: 67px;
  left: 67px;
  width: 54px;
  height: 54px;
  border: solid 6px #3f3f3f;
  border-radius: 50%;
  background: white;
  z-index: 1;
  box-shadow: 0 0 15px -2px #c62828 inset;
  animation: button 3s ease infinite;
}

@-webkit-keyframes roll {
  from {
    transform: rotate(0);
  }
  90%,
  to {
    transform: rotate(720deg);
  }
}

@-webkit-keyframes button {
  from,
  50%,
  to {
    box-shadow: 0 0 15px -2px #c62828 inset;
  }

  25%,
  75% {
    box-shadow: 0 0 10px -2px #1300ea inset;
  }
}

.pokemon {
  height:100%;
  width:80%;
  min-width:910px;
  background-color:white;
  border-radius:10px 10px 10px 10px;
  border: solid 2px black;
  font-family:Minecraft;
  margin: 0 auto;
  padding:1%;
  display:flex;
  flex-direction:row;
  justify-content:space-between;
  padding-left:2%;
  color:black;
}

.select {
  margin-bottom:1%;
}

.cross {
  padding:3px;
  border-radius:4px 4px 4px 4px;
  border: 2px solid black;
  cursor:pointer;
  background-color:red;
  float:top;
}

.cross:hover {
  background-color:#e50000;
}

.name {
  margin-right:5%;
}

.flex-pokemon {
  display:flex;
  flex-direction:row;
  justify-content:flex-start;
}

.name-id {
  font-size:40px;
}

.chara {
  display:flex;
  flex-direction:column;
  justify-content:space-between;
  margin-top:5%;
  margin-left:8%;
  border-radius:8px 8px 8px 8px;
  border:2px solid black;
  padding:2%;
  background-color:#F5F5F5;
}

.pokemon-type-normal {
    background-color:#A8A77A;
}

.pokemon-type-fire {
    background-color:#EE8130;
}

.pokemon-type-water {
    background-color:#6390F0;
}

.pokemon-type-electric {
    background-color:#F7D02C;
}

.pokemon-type-grass {
    background-color:#7AC74C;
}

.pokemon-type-ice {
    background-color:#96D9D6;
}

.pokemon-type-fighting {
    background-color:#C22E28;
}

.pokemon-type-poison {
    background-color:#A33EA1;
}

.pokemon-type-ground {
    background-color:#E2BF65;
}

.pokemon-type-flying {
    background-color:#A98FF3;
}

.pokemon-type-psychic {
    background-color:#F95587;
}

.pokemon-type-bug {
    background-color:#A6B91A;
}

.pokemon-type-rock {
    background-color:#B6A136;
}

.pokemon-type-ghost {
    background-color:#735797;
}

.pokemon-type-dragon {
    background-color:#6F35FC;
}

.pokemon-type-dark {
    background-color:#705746;
}

.pokemon-type-steel {
    background-color:#B7B7CE;
}

.pokemon-type-fairy {
    background-color:#D685AD;
}

.pokemon-type-bug,
.pokemon-type-dark,
.pokemon-type-dragon,
.pokemon-type-electric,
.pokemon-type-fairy,
.pokemon-type-fighting,
.pokemon-type-fire,
.pokemon-type-flying,
.pokemon-type-ghost,
.pokemon-type-grass,
.pokemon-type-ground,
.pokemon-type-ice,
.pokemon-type-normal,
.pokemon-type-poison,
.pokemon-type-psychic,
.pokemon-type-rock,
.pokemon-type-steel,
.pokemon-type-water {
    border-radius:5px 5px 5px 5px;
    text-align:center;
    padding:2%;
    width:40%;
}

</style>