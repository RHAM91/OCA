<template>
    <div class="cuerpo_reconsulta">
        <div class="banner_reconsulta" style="background-color: #243141;color: white;">
            <h2>
                Reconsultas
            </h2>
            <b-button type="button" size="sm" variant="outline-danger" class="btn_flot" @click="salir">Cerrar</b-button>
        </div>
        <div class="cuadro_reconsulta">
            <div class="main_reconsultas" style="width: 80%;">                
                <b-container fluid>
                    <b-row>
                        <b-col sm="12" class="mt-3">
                            <table class="table table-sm table-striped table-bordered" style="font-size: 14px;">
                                <thead>
                                    <tr>
                                        <th style="width: 15%;">
                                            Código
                                        </th>
                                        <th style="width: 55%;">
                                            Consulta
                                        </th>
                                        <th style="text-align: center;width: 15%;">
                                            Costo
                                        </th>
                                        <th style="text-align: center;width: 15%;">
                                            Fecha
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in reconsultas" :key="index">
                                        <td>
                                            {{item.codigo}}
                                        </td>
                                        <td>
                                            {{item.consulta}}
                                        </td>
                                        <td style="text-align:center;">
                                            Q{{item.costo}}
                                        </td>
                                        <td style="text-align: center;">
                                            {{item.fecha}}
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </b-col>
                    </b-row>
                </b-container>
            </div>
        </div>

    </div>
</template>

<script>

import { IP, PUERTO } from '@/config/parametros'
import axios from 'axios'


export default {
    name: "HistorialReconsultas",
    props: ['id'],
    data() {
        return {
            reconsultas: []
        }
    },
    methods: {
        salir(){
            this.$emit('cerrar_modal', false)
        },
        async getDatos(){
            let info = await axios.get(`http://${IP}:${PUERTO}/api/reconsultas/${this.id}`, this.$store.state.token)
            this.reconsultas = info.data
        }
    },
    mounted() {
        this.getDatos()
    },
}
</script>

<style>

</style>