<template>
    <div>
        <button @click="togglePosts">
            Toggle Posts
        </button>
        <ul v-show="displayPosts">
            <li v-for="post in posts" :key="post.id">
                {{ post.title }}
            </li>
        </ul>
        <br/><br/>
        <form>
            <h2>Add Post</h2>
            <input placeholder="title" v-model="form.title">
            <span class="text-danger" v-if="form.errors.has('title')">
                {{ form.errors.first('title') }}
            </span>
            <br/><br/>
            <textarea placeholder="Content" v-model="form.content">

            </textarea>
            <span class="text-danger" v-if="form.errors.has('content')">
                {{ form.errors.first('content') }}
            </span>
            <br/><br/>
            <button type="Button" @click="addPost">
                Add
            </button>
        </form>
    </div>
</template>

<script>
    import Form from 'form-backend-validation';

    export default {
        props: ['name', 'age'],
        data () {
            return {
                displayPosts: true,
                posts: [],
                form: new Form({
                    title: null,
                    content: null
                })
            };
        },
        mounted() {
            this.loadPosts();
        },
        methods: {
            togglePosts() {
                this.displayPosts = !this.displayPosts;
            },
            addPost() {
                this.form.post('/posts')
                    .then(response => {
                        this.loadPosts();
                    })
                    .catch(response => {

                    });
            },
            loadPosts() {
                axios.get('/posts').then(response => {
                    this.posts = response.data;
                });
            }
        }
    }
</script>
