<template>
    <div id="app">
        <p>Имя:
            <br>
            <input v-model="currentNotes.fName">
            </p>
        <p>Фамилия:
            <br>
            <input v-model="currentNotes.sName">
            </p>
        <p>Отчество:
            <br>
            <input v-model="currentNotes.tName">
            </p>
        <p>Номер телефона:
            <br>
            <input v-model="currentNotes.phone">
            </p>
        <p>Избранный?
            <input type="checkbox" v-model="currentNotes.favorite">
            </p>
        <p>По имени
            <input value="name" type="radio" v-model="sort" v-on:click="toggleSorting('name')">
            </p>
        <p>По Фамилии
            <input value="sec_name" type="radio" v-model="sort" v-on:click="toggleSorting('sec_name')">
        </p>
        <p><button v-on:click="toggleConfirm()">Сохранить</button></p>
        <li v-for="(item, index) in notes" v-bind:key=index>{{item.fName}} {{item.tName}} {{item.sName}} {{item.phone}} 
            <span v-if="item.favorite"> Избранный</span>
            <button v-on:click="toggleChange(index)">Изменить</button>
            <button v-on:click="toggleDelete(index)">Удалить</button>
        </li>

    </div>
</template>

<script>
    export default {
        name: "nodesBook",
        data() {
            return {
                state: -1,
                sort: "name",
                notes: [],
                currentNotes: {
                    fName: "",
                    sName: "",
                    tName: "",
                    phone: "",
                    favorite: false
                },

                currentID: -1
            }
        },
        methods: {
            async toggleConfirm() {
                if (this.currentNotes.phone !== "") {
                    if (this.state === -1) {
                        this.notes.push({
                            fName: this.currentNotes.fName,
                            sName: this.currentNotes.sName,
                            tName: this.currentNotes.tName,
                            phone: this.currentNotes.phone,
                            favorite: this.currentNotes.favorite
                        });
                        try{
                            await this.$http.post('http://localhost:3000/notes', this.currentNotes);
                            this.update();
                        }catch(err){
                            console.log(err);
                        }
                    } else {
                        this.notes[this.state] = {
                            id: this.currentNotes.id,
                            fName: this.currentNotes.fName,
                            sName: this.currentNotes.sName,
                            tName: this.currentNotes.tName,
                            phone: this.currentNotes.phone,
                            favorite: this.currentNotes.favorite
                        };
                        try{
                            console.log(this.notes[this.state]);
                            await this.$http.put('http://localhost:3000/notes/'+ this.currentID, this.notes[this.state]);
                            this.update();
                        }catch(err){
                            console.log(err);
                        }
                        this.state = -1;
                    }
                    this.toggleSorting(this.sort);
                    this.currentNotes.fName = "";
                    this.currentNotes.sName = "";
                    this.currentNotes.tName = "";
                    this.currentNotes.phone = "";
                    this.currentNotes.favorite = "";
                } else {
                    alert("Пустые данные!!");
                }
            },
            toggleChange (id) {
                this.currentNotes = {
                    fName: this.notes[id].fName,
                    sName: this.notes[id].sName,
                    tName: this.notes[id].tName,
                    phone: this.notes[id].phone,
                    favorite: this.notes[id].favorite
                };
                this.state = id;
                this.currentID = this.notes[id].id;
            },
            async toggleDelete (id) {
                await this.$http.delete('http://localhost:3000/notes/' + this.notes[id].id);
                this.update();
            },
            toggleSorting (value) {
                if (value === "name") this.notes.sort((a, b) => a.fName < b.fName ? 1 : -1);
                else if (value === "sec_name") this.notes.sort((a, b) => a.sName < b.sName ? 1 : -1);
                this.notes.sort((a, b) => a.favorite < b.favorite ? 1 : -1);
            },
            async update() {
                try {
                    await this.$http.get('http://localhost:3000/notes').then((res) => res.json()).then((res) => (this.notes = res));
                    this.toggleSorting(this.sort);
                } catch (err) {
                    console.error(err);
                }
            },
        },
        created() {
            this.toggleSorting(this.sort);
            this.update();
        }
    }
</script>

<style scoped>
button {
    cursor: pointer;
    min-width: 200px;
    padding: 10px 5px;
    text-transform: uppercase;
    vertical-align: middle;
    background: #3da35a;
    border: none;
    color: #ffffff;
    font-weight: 200;
    border-radius: 5px;
    box-shadow: 0 0 10px rgb(56,100,38);
  }

input {
    color: #333;
    padding: 1rem 1rem;
    border-radius: 0.2rem;
    background-color: rgb(255, 255, 255);
    border: none;
    width: 10%;
    border-bottom: 0.3rem solid transparent;
}

  
li {
    padding: 10px;
}
</style>