<template>
    <div id="app">

        <app-header :readSearch = "readSearch" />

        <div class="container">
            <h1 class="pt-3 pb-3">Персонажи Marvel</h1>

            <app-modal :character = "searchCharacters[characterIndex]"/>

            <spinner v-if="loading" />

            <div v-else class="row">
                <h5 v-if="!searchCharacters.length">Ничего не найдено</h5>
                <div v-else
                    v-for="(element, index) in searchCharacters"
                    :key="element.id"
                    class="card mb-3 col-sm-12 col-md-6 col-lg-4"
                >
                    <div class="row g-0">
                        <div class="col-md-4">
                            <img :src="element.thumbnail"
                                class="img-fluid rounded-start"
                                :alt="element.thumbnail"
                            >
                        </div>
                        <div class="col-md-8">
                            <div class="card-body">
                                <h5 class="card-title">{{element.name}}</h5>
                                <button
                                    type="button"
                                    class="btn btn-secondary btn-sm"
                                    data-bs-toggle="modal"
                                    data-bs-target="#exampleModal"
                                    @click="characterIndex = index">
                                    Подробнее
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";

    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner,
        },
        data() {
            return {
                loading: false,
                characters: [],
                characterIndex: 0,
                search: '',
            }
        },
        methods: {
            fetchCharacters: function(){
                return fetch('https://netology-api-marvel.herokuapp.com/characters')
                        .then(res => res.json())
                        .then(json => this.characters = json)
            },
            readSearch: function(string){
                this.search = string
            },
        },
        computed: {
            searchCharacters: function(){
                const {search, characters} = this
                return characters.filter(character => {
                    return character.name.toLowerCase().indexOf(search.toLowerCase()) !== -1
                })
            },
        },
        async mounted(){
            this.loading = true
            await this.fetchCharacters()
            this.loading = false
        },
    }
</script>

<style>

</style>
