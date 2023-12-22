<template>
    <div class="row">
        <div class="col-md-12">
            <RouterLink class="btn btn-primary" to="/museo_crear">agregar del museo</RouterLink>
        </div>
        <div class="col-md-12">
            <div class="card">
                <div class="card-body">
                    <table class="table">
                        <thead>
                            <tr>
                                <th scope="col">nombre</th>
                                <th scope="col">sigla</th>
                                <th scope="col">personalidad</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="data in museo">
                                <td>{{ data.nombre }}</td>
                                <td>{{ data.sigla }}</td>
                                <td>{{ data.personalidad }}</td>
        
                                <td>
                                    <RouterLink class="btn btn-success" :to="'/museo_editar/' + data.id">Editar</RouterLink>
                                    <button class="btn btn-danger" @click="eliminar(data.id)">Eliminar
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import { collection, getDocs, query, where,deleteDoc,doc } from "firebase/firestore";
import { auth, db } from "../../firebaseConfig";

import { defineComponent } from 'vue';
export default defineComponent({
    data() {
        return {
            loadingDoc: false,
            museo: []
        }
    },

    methods: {
        async getUrls() {
            try {
                this.museo= []
                const q = query(
                    collection(db, "museo")
                );
                const querySnapshot = await getDocs(q);
                querySnapshot.forEach((doc) => {
                    // console.log(doc.id);
                    this.museo.push({
                        nombre: doc.data().nombre,
                        sigla: doc.data().sigla,
                        personalidad: doc.data().personalidad,
                        id: doc.id,
                        // ...doc.data()
                    });
                });
                // console.log(this.curso);
            } catch (error) {
                console.log(error);
            }
        },
        async eliminar(id) {
            try {
                const q = doc(db, "museo", id);
                const docRef = await deleteDoc(q);
                this.getUrls()
            } catch (error) {
                console.log(error);
            }
        },
    },
    mounted() {
        this.getUrls()
    }
})
</script>