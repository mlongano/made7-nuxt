<script setup lang="ts">
const { data:vociRaw , error:errorOnFetchVoci   } = await useFetch('http://localhost:1337/api/voci-del-menu?populate=categoria%2Ccaratteristiche%2Callergeni');
const { data:caratteristicheRaw , error:errorOnFetchCaratteristiche   } = await useFetch('http://localhost:1337/api/caratteristiche?populate=icona');
const { data:categorieRaw , error:errorOnFetchCategorie   } = await useFetch('http://localhost:1337/api/categorie');
const { data:allergeniRaw , error:errorOnFetchAllergeni   } = await useFetch('http://localhost:1337/api/allergeni');
// const voci = await json.map(v => ({
//   id: v.id,
//   nome: v.nome,
//   categoria: v.categoria.nome,
//   caratteristiche: v.caratteristiche.map(c => c.nome),
//   callergeni: v.callergeni.map(c => c.nome),
// }))
</script>

<script lang="ts">
export default {
  computed: {
    voci() {
      return this.vociRaw.data.map(v => ({
        id: v.id,
        nome: v.attributes?.nome,
        categoria: v.attributes.categoria.data.attributes.nome,
        caratteristiche: v.attributes.caratteristiche.data?.map(c => c.attributes),
        allergeni: v.attributes.allergeni.data?.map(a => a.attributes),
      }));
    },
    caratteristiche() {
      return this.caratteristicheRaw.data.map(c => ({
        id: c.id,
        nome: c.attributes.nome,
        icona: c.attributes.icona.data.attributes.url,
      }));
    },
    categorie() {
      return this.categorieRaw.data.map(c => ({
        id: c.id,
        nome: c.attributes.nome,
      }));
    },
    allergeni() {
      return this.allergeniRaw.data.map(a => ({
        id: a.id,
        nome: a.attributes.nome,
      }));
    },
  }
}
</script>
<template>
  <div class="text-gray-400">
    <div v-if="vociRaw" class="font-medium text-gray-500"><h2 class="title">Voci del menu</h2><p v-for="v in voci" :key="v.id">{{ v.nome }}</p></div>
    <div v-if="errorOnFetchVoci" class="font-medium text-red-800"><h2 class="title">Errore nel contattare il server</h2><p>Non riesco ad ottenere le voci del menu</p></div>
    <div v-if="caratteristicheRaw" class="font-medium text-gray-500"><h2 class="title">Caratteristiche</h2><p class="flex items-center justify-center" v-for="c in caratteristiche" :key="c.id"><img  class="h-4 mr-2" :src="`http://localhost:1337${c.icona}`" :alt="c.nome">  {{ c.nome }}</p></div>
    <div v-if="errorOnFetchCaratteristiche" class="font-medium text-red-800"><h2 class="title">Errore nel contattare il server</h2><p>Non riesco ad ottenere le caratteristiche</p></div>
    <div v-if="categorieRaw" class="font-medium text-gray-500"><h2 class="title">Categorie</h2><p v-for="c in categorie" :key="c.id">{{ c.nome }}</p></div>
    <div v-if="errorOnFetchCategorie" class="font-medium text-red-800"><h2 class="title">Errore nel contattare il server</h2><p>Non riesco ad ottenere le categorie</p></div>
    <div v-if="allergeniRaw" class="font-medium text-gray-500"><h2 class="title">Allergeni</h2><p v-for="a in allergeni" :key="a.id">{{ a.nome }}</p></div>
    <div v-if="errorOnFetchAllergeni" class="font-medium text-red-800"><h2 class="title">Errore nel contattare il server</h2><p>Non riesco ad ottenere gli allergeni</p></div>
  </div>
</template>

<style scoped lang="postcss">
.title {
  @apply capitalize text-2xl text-indigo-500 mt-4;
}
</style>
