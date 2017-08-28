<template>
    <div class='row'>
        <h1>Notícias</h1>
        <h4>Nova Notícia</h4>
        <form action="#" @submit.prevent="createNews()">
            <div class="input-group">
                <input v-model="news.title" type="text" name="title" class="form-control" autofocus>
            </div>
            <div class="input-group">
                <input v-model="news.body" type="text" name="body" class="form-control">
            </div>
            <div class="input-group">
                <button type="submit" class="btn btn-primary">Cadastrar</button>
            </div>
        </form>
        <h4>Todas notícias</h4>
        <ul class="list-group">
            <li v-if='list.length === 0'>Não existem notícias cadastradas ainda!</li>
            <li class="list-group-item" v-for="(news, index) in list">
                 {{ news.title }}
                 <button @click="deleteNews(news.id)" class="btn btn-danger btn-xs pull-right">Delete</button>
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                list: [],
                news: {
                    id: '',
                    body: ''
                }
            };
        },
        
        created() {
            this.fetchNewsList();
        },
        
        methods: {
            fetchNewsList() {
                axios.get('api/news').then((res) => {
                    this.list = res.data;
                });
            },
 
            createNews() {
                axios.post('api/news', this.news)
                    .then((res) => {
                        this.news.body = '';
                        this.edit = false;
                        this.fetchNewsList();
                    })
                    .catch((err) => console.error(err));
            },
 
            deleteNews(id) {
                axios.delete('api/news/' + id)
                    .then((res) => {
                        this.fetchNewsList()
                    })
                    .catch((err) => console.error(err));
            },
        }
    }
</script>
</script>