<template>
    <main>
        <div
            v-if="isModalVisible"
            class="overlay"
        >
            <div class="modal">
                <textarea
                    v-model.trim="newNote.text"
                    name="note"
                    id="note"
                    cols="30"
                    rows="10"
                >
                </textarea>

                <p
                    v-if="errorMessage"
                    class="error"
                >
                    {{ errorMessage }}
                </p>

                <button @click="addNote">Add Note</button>
                <button
                    @click="updateModalVisibility(false)"
                    class="close"
                >
                    Close
                </button>
            </div>
        </div>

        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="updateModalVisibility(true)">+</button>
            </header>

            <div class="cards-container">
                <div
                    v-for="(note, index) in notes"
                    :key="index"
                    class="card"
                    :style="{ backgroundColor: note.backgroundColor }"
                >
                    <p class="main-text">{{ note.text }}</p>

                    <p class="date">{{ note.date }}</p>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup lang="ts">
    import { ref } from "vue";

    const getRandomColor = () => {
        return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
    };

    const isModalVisible = ref(false);
    const updateModalVisibility = (visibility: boolean) => {
        isModalVisible.value = visibility;
    };
    
    type Note = {
        id: number,
        text: string,
        date: string,
        backgroundColor: string,
    };

    const genNote = () => {
        return {
            id: Math.floor(Math.random() * 10000),
            text: "",
            date: new Date().toLocaleDateString(),
            backgroundColor: getRandomColor(),
        };
    };

    const newNote = ref(genNote());

    const notes = ref([] as Note[]);

    const errorMessage = ref("");

    const clearError = () => {
        errorMessage.value = "";
    };

    const addNote = () => {
        if (newNote.value.text.length < 10) {
            return errorMessage.value = "Note needs to be at least 10 characters long.";
        }
        notes.value.push(newNote.value);
        newNote.value = genNote();
        clearError();
        updateModalVisibility(false);
    };
</script>

<style scoped>
    body {
        height: 100%;
        width: 100%;
    }

    .container {
        max-width: 1000px;
        padding: 10px;
        margin: 0 auto;
    }

    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    h1 {
        font-weight: bold;
        margin-bottom: 25px;
        font-size: 72px;
    }

    header button {
        border: none;
        padding: 10px;
        width: 50px;
        height: 50px;
        cursor: pointer;
        background-color: #000;
        border-radius: 100%;
        color: #fff;
        font-size: 20px;
    }

    .card {
        width: 225px;
        height: 225px;
        background-color: rgb(237, 182, 44);
        padding: 10px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin-right: 20px;
        margin-bottom: 20px;
    }

    .cards-container {
        display: flex;
        flex-wrap: wrap;
    }

    .date {
        font-size: 13px;
        font-weight: bold;
    }

    .overlay {
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: rgb(0, 0, 0, 0.77);
        z-index: 10;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modal {
        width: 750px;
        background-color: #fff;
        padding: 30px;
        border-radius: 10px;
        position: relative;
        display: flex;
        flex-direction: column;
    }

    .modal button {
        padding: 10px 20px;
        font-size: 20px;
        width: 100%;
        background-color: aqua;
        border: none;
        color: #fff;
        cursor: pointer;
        margin-top: 15px;
    }

    .modal .close {
        background-color: brown;
        margin-top: 10px;
    }

    .modal .error {
        color: red;
    }
</style>