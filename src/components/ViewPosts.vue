<script setup>
    import axios from 'axios';
</script>

<script>
export default {
    data() {
        return {
            moods: ["Happy", "Sad", "Angry"],
            posts: [],
            entry: "",
            mood: "",
            showEditPost: false,
            editPostId: "",
        }
    },
    computed: {
        baseUrl() {
            if (window.location.hostname=='localhost')
                return 'http://localhost:3000' 
            else {
                const codespace_host = window.location.hostname.replace('5173', '3000')
                return `https://${codespace_host}`;
            }
        }
    },
    created() {
        axios.get(`${this.baseUrl}/posts`)
            .then(response => {
                this.posts = response.data
            })
            .catch(error => {
                this.posts = [{ entry: 'There was an error: ' + error.message }]
            })
    },
    methods: {
        editPost(id) {
            const post = this.posts.find(p => p.id === id);
            if (post) {
                this.entry = post.entry;
                this.mood = post.mood;
                this.editPostId = id;
                this.showEditPost = true;
            }
        },
        updatePost(event) {
            event.preventDefault();
            const updatedPost = {
                entry: this.entry,
                mood: this.mood
            };
            
            axios.post(`${this.baseUrl}/updatePost`, {
                id: this.editPostId,
                ...updatedPost
            })
            .then(response => {
                const index = this.posts.findIndex(p => p.id === this.editPostId);
                if (index !== -1) {
                    this.posts[index] = { ...this.posts[index], ...updatedPost };
                }
                this.showEditPost = false;
                this.entry = "";
                this.mood = "";
                this.editPostId = "";
            })
            .catch(error => {
                console.error('Error updating post:', error);
                alert('Error updating post: ' + error.message);
            });
        }
    }
}
</script>

<template>
    <div id="demo">
        <h2> View Blog Posts </h2>
        <table class="table m-2">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Entry</th>
                    <th>Mood</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="post in posts">
                    <td>{{ post.id }}</td>
                    <td>{{ post.entry }}</td>
                    <td>{{ post.mood }}</td>
                    <td><button @click="editPost(post.id)">Edit</button></td>
                </tr>
            </tbody>

        </table>
        <!-- TODO Show form for editing post only when edit button is clicked -->
        <div id="editPost" v-if="showEditPost">
            <h3>Edit Post</h3>
            <div id="postContent" class="mx-3">
                <form @submit="updatePost">
                    <div class="mb-3">
                        <label for="entry" class="form-label">Entry</label>
                        <textarea id="entry" class="form-control" v-model="entry" required></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="mood" class="form-label">Mood</label>
                        <select id="mood" class="form-select" v-model="mood" required>
                            <option value="" disabled>Select Mood</option>
                            <option v-for="mood in moods" :value="mood">{{ mood }}</option>
                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">Update Post</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
