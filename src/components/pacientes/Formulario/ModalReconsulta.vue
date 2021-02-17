<template>
    <div class="cuerpo_reconsulta">
        <div class="banner_reconsulta">
            <div class="logo_banner">
                <img src="@/assets/reload.png" width="100%;" alt="">
            </div>
            <h2>
                Reconsultas
            </h2>
            <b-button type="button" size="sm" variant="outline-danger" class="btn_flot" @click="salir">Cerrar</b-button>
        </div>
        <div class="cuadro_reconsulta">
            <div class="main_reconsultas">
                <h2 style="border-bottom: 1px solid black;color: rgb(136,173,40);">
                    Ingreso de reconsultas
                </h2>
                <b-container fluid>
                    <b-row>
                        <b-col sm="12" class="mt-4">
                            <table class="table table-sm table-striped table-bordered" style="font-size: 14px;">
                                <thead>
                                    <tr>
                                        <th style="width: 30%;">
                                            Elemento
                                        </th>
                                        <th style="width: 70%;">
                                            Detalle
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr>
                                        <td>
                                            Paciente
                                        </td>
                                        <td>
                                            {{nombre}}
                                        </td>
                                    </tr>
                                    <tr>
                                        <td>
                                            Código paciente
                                        </td>
                                        <td>
                                            {{idx}}
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </b-col>
                        <b-col sm="12" class="mt-3">
                            <label>Consulta por:</label>
                            <b-form-textarea
                                id="textarea"
                                v-model="descripcion"
                                placeholder="Descripción"
                                rows="3"
                                max-rows="6"
                            ></b-form-textarea>
                        </b-col>
                        <b-col sm="10" class="mt-3">
                            <label>Consto de la consulta</label>
                            <b-form-input type="number" step="0.01" placeholder="Q" size="sm"></b-form-input>
                        </b-col>
                        <b-col sm="2" class="mt-5 d-flex flex-row-reverse">
                            <b-button type="button" block size="sm" variant="primary">Grabar</b-button>
                        </b-col>
                        <b-col sm="12" class="mt-5" style="text-align: center;">
                            <b-button type="button" size="sm" variant="primary" style="margin-right: 15px;" @click="abrirHistorial">Ver reconsultas</b-button>
                            <b-button type="button" size="sm" variant="primary" @click="abrirGenerarODT">Generar ODT</b-button>
                        </b-col>
                    </b-row>
                </b-container>
            </div>
        </div>

        <Historial v-if="historial" :id="idx" v-on:cerrar_modal="cerrarHistorial" />
        <ModalGenerarODTx v-if="ModalGenerarODT" :info="idx" v-on:cerrar_modal="cerarGenerarODT" />

    </div>
</template>

<script>
import { mapState } from 'vuex'
import Historial from './HistorialReconsultas.vue'
import ModalGenerarODTx from './ModalGenerarODT_1.vue'

export default {
    name: 'Reconsulta',
    props: ['idx'],
    computed: {
        ...mapState(['pacientes'])
    },
    components:{
        Historial,
        ModalGenerarODTx
    },
    data() {
        return {
            nombre: '',
            descripcion: '',
            historial: false,
            ModalGenerarODT: false,
            info_paciente: ''
        }
    },
    methods: {
        salir(){
            this.$emit('cerrar_modal', false)
        },
        getDatos(){
            let filtro = this.pacientes.filter(paciente => paciente._id == this.idx)
            this.nombre = filtro[0].nombre
        },
        abrirHistorial(){
            this.historial = true
        },
        cerrarHistorial(){
            this.historial = false
        },
        abrirGenerarODT(){
            this.ModalGenerarODT = true
        },
        cerarGenerarODT(){
            this.ModalGenerarODT = false
        },
    },
    mounted() {
        this.getDatos()
    },
}
</script>

<style>
    .cuerpo_reconsulta{
        width: 100%;
        height: 100vh;
        background-color: white;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 999;
    }
        .banner_reconsulta{
            width: 100%;
            height: 80px;
            background-color: white;
            border-bottom: 1px solid rgb(230,230,230);
            display: flex;
            align-items: center;
            padding-left: 10px;
        }
            .logo_banner{
                width: 50px;
                height: 50px;
                margin-right: 15px;
            }

        .cuadro_reconsulta{
            width: 100%;
            height: calc(100% - 80px);
            background-color: #f3f3f3;
            display: flex;
            justify-content: center;
            padding-top: 25px;
            padding-bottom: 25px;
        }
            .main_reconsultas{
                background-color: white;
                width: 70%;
                border: 1px solid rgba(204,204,204, 0.3);
                padding: 10px;
            }

    .btn_flot{
        position: fixed;
        right: 15px;
        top: 25px;
    }
</style>