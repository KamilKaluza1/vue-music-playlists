<template>
    <div class="add-song" >
        <button v-if="!showForm" @click='showForm=true'>Add Songs</button>
        <button v-if="showForm" @click='showForm=false'>Close</button>
        <form v-if="showForm" @submit.prevent="handleSubmit">
            <h4>Add a new song</h4>
            <input type="text" required placeholder="Song title" v-model="title">
            <input type="text" required placeholder="Artist" v-model="artist">
            <button>Add</button>
        </form>
    </div>
</template>

<script>
import { ref } from 'vue';
import useDocument from '@/composables/useDocument';

export default{
    props:['document'],
    setup(props){
        const title = ref('')
        const artist = ref('')
        const showForm = ref(false)
        const {updateDoc} = useDocument('playlists', props.document.id )


        const handleSubmit = async() =>{
            const newSong = {
                title: title.value,
                artist: artist.value,
                id: Math.floor(Math.random() * 99999999999999)
            }
            await updateDoc({
                songs:[...props.document.songs, newSong]
            })
            title.value = ''
            artist.value = ' '
            console.log(newSong)

        }

    
    
    
        return {artist, title, showForm, handleSubmit}
    }
}

</script>