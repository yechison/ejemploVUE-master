<template>
    <div class="row">
        <div class="col-md-12">
            <div class="card">
                <div class="card-body">
                    <div class="mb-3">
                        <label class="form-label">nombre institucion(*)</label>
                        <input v-model="museo.nombre" type="text" class="form-control">
                    </div>
                    <div class="row">
                        <div class="col-md-3">
                            <div class="mb-3">
                                <label class="form-label">sigla</label>
                                <input v-model="museo.sigla" type="text" class="form-control">
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="mb-3">
                                <label class="form-label">rut personalidad juridica</label>
                                <input v-model="museo.personalidad" type="text" class="form-control">
                            </div>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">director o encargado(*)</label>
                        <input v-model="museo.encargado" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">director del museo</label>
                        <input v-model="museo.director" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">region/provincia/comuna</label>
                        <input v-model="museo.region" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">coordenadas geograficas</label>
                        <input v-model="museo.coordenadas" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">correo electronico del museo</label>
                        <input v-model="museo.correo" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">telefono del museo</label>
                        <input v-model="museo.telefono" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">sitio web del museo</label>
                        <input v-model="museo.sitioweb" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">imagen del museo</label>
                        <input v-model="museo.imagen" type="text" class="form-control">
                    </div>
                </div>
            </div>
        </div>
        <div class="col-md-12">
            <button @click="guardar()" type="button" class="btn btn-primary">Guardar</button>
            <RouterLink class="btn btn-light" to="/museo_listar">Cancelar</RouterLink>
        </div>
    </div>
</template>
<script>

import { collection, getDocs,getDoc, query, where, addDoc, updateDoc, doc } from "firebase/firestore";
import { auth, db } from "../../firebaseConfig";
import { defineComponent } from 'vue';
export default defineComponent({
    data() {
        return {
            id: this.$route.params.id,
            museo: {
                nombre: '',
                sigla: '',
                personalidad: '',
                encargado: '',
                director: '',
                region: '',
                coordenadas: '',
                correo: '',
                telefono: '',
                sitioweb: '',
                imagen: '', 
            },
        }
    },
    methods: {
        async getUrls() {
            try {
                const q = doc(db, "museo", this.id);
                const respuesta = await getDoc(q);
                this.museo=respuesta.data()
            } catch (error) {
                console.log(error);
            }
        },
        async guardar() {
            try {
                const q = doc(db, 'museo',this.id);
                const docRef = await updateDoc(q,this.museo);
                this.$router.push({ name: 'museo_listar' })
            } catch (error) {
                console.log(error);
            }
        },
    },
    created() {
        this.getUrls()
    }
})
</script>