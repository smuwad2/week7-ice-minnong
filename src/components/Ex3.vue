<script>
    import axios from 'axios';

    export default { 
        data() {
            return {
                subject: '',
                entry: '',
                mood: '',
                moods: ['Happy', 'Sad', 'Angry']
            }
        },
        methods: {
            async submitPost() {
                try {
                    const response = await axios.post('http://localhost:3000/addPost', {
                        subject: this.subject,
                        entry: this.entry,
                        mood: this.mood
                    });
                    
                    if (response.status === 200) {
                        alert('Post added successfully!');
                        // Clear the form
                        this.subject = '';
                        this.entry = '';
                        this.mood = '';
                    }
                } catch (error) {
                    console.error('Error adding post:', error);
                    alert('Failed to add post. Please try again.');
                }
            }
        }
    }
</script>

<template>
    <div class="table m-2">
        <h3>Add a New Blog Post</h3>

        Subject: <input type='text' size='30' v-model='subject' required>
        <br>

        Entry: <br>
        <textarea name='entry' cols='80' rows='5' v-model='entry' required></textarea>
        <br>

        Mood:
        <select v-model="mood" required>
            <option value="" disabled selected>Select a mood</option>
            <option v-for="moodOption in moods" :key="moodOption" :value="moodOption">
                {{ moodOption }}
            </option>
        </select>

        <br>

        <br>
        <button @click="submitPost">Submit New Post</button>

        <hr> Click  <a><router-link to="/ViewPosts/">here</router-link></a>  to return to Main Page
       
    </div>
</template>

