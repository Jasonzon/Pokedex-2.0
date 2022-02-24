<template>
  <div class="select">
    <span>Search by Name or ID:</span>
    <input class="input" :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" placeholder='Pokemon/ID' />
    <select class="select-idsort" :value="idValue" @input="$emit('update:idValue', $event.target.value)">
      <option :value="true">Sort by id</option>
      <option :value="false">Sort by name</option>
    </select>
    <select class="select-reverse" :value="reverseValue" @input="$emit('update:reverseValue', $event.target.value)">
      <option :value="true">Ascending</option>
      <option :value="false">Descending</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'SelectComponent',
  props: {
    modelValue: {
      type: String,
      required: true
    },
    idValue: {
      type: String,
      required: true
    },
    reverseValue: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.select {
  background-color:#101010;
  width:100%;
  height:100px;
  color:white;
  padding:10px;
  font-family:Minecraft;
}

.select-idsort, .select-reverse {
  margin-left:2%;
  padding:0.5%;
}

.input {
  margin-top:3%;
  font-family:Minecraft;
  margin-left:1%;
}
</style>
