<template>
    <div class="modal-overlay" v-if="isOpen">
        <div class="modal-content">
            <h2 class="modal-title">Reporte por fecha de nacimiento</h2>
            <p class="modal-subtitle">Ingresa los siguientes datos para generar tu reporte</p>

            <div class="input-group">
                <label for="reportName">Descripción del reporte</label>
                <input type="text" id="reportName" v-model="tittle" placeholder="Escribe aquí..." class="input-text"/>
            </div>

            <div class="input-group">
                <label for="startDate">Fecha de nacimiento</label>
            </div>

            <div class="date-inputs">
                <div class="input-group">
                    <label for="startDate">Inicio</label>
                    <input type="date" id="startDate" v-model="start_date" class="input-date"/>
                </div>
                <div class="input-group">
                    <label for="endDate">Fin</label>
                    <input type="date" id="endDate" v-model="end_date" class="input-date"/>
                </div>
            </div>

            <button class="btn btn-cancel" @click="$emit('close')">Cancelar</button>
            <button class="btn" :disabled="!isFormValid" @click="generateReport">Generar reporte</button>
        </div>
    </div>
</template>

<script>
    import axios from '../axios';

    export default {
        props: {
            isOpen: Boolean,
        },
        data() {
            return {
                tittle: "",
                start_date: "",
                end_date: "",
            };
        },
        computed: {
            isFormValid() {
                return this.tittle.trim() !== "" && this.start_date !== "" && this.end_date !== "";
            }
        },
        methods: {
            async generateReport() {
                if (!this.isFormValid) return;

                try {
                    await axios.post('/generate-report', {
                        tittle: this.tittle,
                        start_date: this.start_date,
                        end_date: this.end_date
                    });

                    this.tittle = "";
                    this.start_date = "";
                    this.end_date = "";

                    this.$emit("close");
                    this.$emit("reportGenerated");

                } catch (error) {
                    console.error("Error al generar reporte:", error);
                }
            }
        }
    };
</script>