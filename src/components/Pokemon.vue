<template>
    <div class="pokemon-item">
        <img @load="$emit('loaded')" :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/' + `${id}` + '.svg'" width="200" height="200"/>
        <div class="infos">
            <span class="name">{{name.charAt(0).toUpperCase() + name.slice(1)}}</span>
            <span class="id">#{{id}}</span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'PokemonComponent',
    props: {
        name: {
            type: String,
            required: true
        },
        url: {
            type: String,
            required: true
        },
        id: {
            type: String,
            required: true
        }
    }
}
</script>

<style scoped>
.pokemon-item {
    border-radius: 10px 10px 10px 10px;
    border: 2px solid black;
    background-color:#F8F8F8;
    font-family: "Minecraft";
    cursor:pointer;
    padding:5px;
}

.pokemon-item:hover {
    background-color:#E8E8E8;
}

.name {
    margin-right:3%;
}
</style>
