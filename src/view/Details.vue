<template>
  <div v-if="pokemon" class="details">
    <h1>{{ pokemon.name }}</h1>
    <img :src="pokemon.image" :alt="pokemon.name" class="pokemon-image" />
    
    <h3 class="tipos">Tipos:</h3>
    <div class="types-container">
      <span 
        v-for="t in pokemon.types" 
        :key="t" 
        :class="['type-tag', t]" 
      >
        {{ t }}
      </span>
    </div>

    <div class="stats-container">
      <h3>Estatísticas:</h3>
      <ul>
        <li>
          <span>HP:</span>
          <progress :value="pokemon.stats.hp" max="255"></progress>
          <span>{{ pokemon.stats.hp }}</span>
        </li>
        <li>
          <span>Ataque:</span>
          <progress :value="pokemon.stats.attack" max="255"></progress>
          <span>{{ pokemon.stats.attack }}</span>
        </li>
        <li>
          <span>Defesa:</span>
          <progress :value="pokemon.stats.defense" max="255"></progress>
          <span>{{ pokemon.stats.defense }}</span>
        </li>
        <li>
          <span>Velocidade:</span>
          <progress :value="pokemon.stats.speed" max="255"></progress>
          <span>{{ pokemon.stats.speed }}</span>
        </li>
      </ul>
    </div>

    <button @click="$router.back()">Voltar</button>
  </div>
</template>

<script>
import pokemonService from "../services/pokemonService";


export default {
  props: ["name"],

  data() {
    return {
      pokemon: null
    };
  },

  methods: {
    async fetchPokemonDetails() {
      try {
        const res = await pokemonService.getPokemonByName(this.name);
        const d = res.data;

        // Estrutura garantida para o seu template
        this.pokemon = {
          id: d.id,
          name: d.name,
          image: d.sprites.other["official-artwork"].front_default,
          types: d.types.map(t => t.type.name),
          stats: {
            hp: d.stats.find(s => s.stat.name === "hp").base_stat,
            attack: d.stats.find(s => s.stat.name === "attack").base_stat,
            defense: d.stats.find(s => s.stat.name === "defense").base_stat,
            speed: d.stats.find(s => s.stat.name === "speed").base_stat,
          }
        };
      } catch (error) {
        console.error("Erro ao carregar detalhes do Pokémon:", error);
      }
    }
  },

  mounted() {
    this.fetchPokemonDetails();
  }
};
</script>

<style scoped>


.tipos{
  color: black;
}
.details {
  text-align: center;
  padding: 40px 20px;
  max-width: 600px;
  margin: 20px auto;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.details h1{
  color: black;
}



.pokemon-image {
  width: 250px;
  height: auto;
  margin: 15px 0;
}

/* Estilo para as tags de tipo */
.types-container {
    margin-bottom: 20px;
    color: black;
}

.type-tag {
    display: inline-block;
    padding: 6px 12px;
    margin: 5px;
    border-radius: 20px;
    color: white; /* Texto sempre branco para contraste */
    font-weight: bold;
    text-transform: uppercase;
    font-size: 0.9em;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* Mapeamento de Cores para cada tipo  */

.normal { background-color: #A8A77A; }
.fire { background-color: #EE8130; }
.water { background-color: #6390F0; }
.electric { background-color: #F7D02C; }
.grass { background-color: #7AC74C; }
.ice { background-color: #96D9D6; }
.fighting { background-color: #C22E28; }
.poison { background-color: #A33EA1; }
.ground { background-color: #E2BF65; }
.flying { background-color: #A98FF3; }
.psychic { background-color: #F95587; }
.bug { background-color: #A6B91A; }
.rock { background-color: #B6A136; }
.ghost { background-color: #735797; }
.dragon { background-color: #6F35FC; }
.steel { background-color: #B7B7CE; }
.fairy { background-color: #D685AD; }
.dark { background-color: #705746; }

/* Estilo para Estatísticas */
.stats-container {
    text-align: left;
    margin-top: 30px;
    padding: 0 20px;
    color: rgb(0, 0, 0);
}

.stats-container ul {
    list-style: none;
    padding: 0;
}

.stats-container li {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.stats-container li span:first-child {
    width: 100px; /* Largura fixa para o nome da estatística */
    font-weight: bold;
    margin-right: 15px;
}

.stats-container li progress {
    flex-grow: 1; /* Faz a barra de progresso preencher o espaço restante */
    height: 15px;
    margin-right: 15px;
  
}

progress::-webkit-progress-value {
    background-color: #4CAF50;
}
progress::-moz-progress-bar {
    background-color: #4CAF50;
}
</style>