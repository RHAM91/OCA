<template>
    <div class="contenedor-descripcion">
        <div class="cuerpo-descripcion">
            <div class="cabecera_decripcion">
                <div v-if="!o_completas">
                    Historial de ordenes:
                </div>
                <div v-if="o_completas">
                    Ordenes completas:
                </div>
                <div>
                    <b-button v-if="!o_completas" type="button" size="sm" variant="outline-primary" @click="completas" style="margin-right: 10px;">Completas</b-button>
                    <b-button v-if="o_completas" type="button" size="sm" variant="outline-danger" @click="transito" style="margin-right: 10px;">Transito</b-button>
                    <b-button type="button" size="sm" variant="light" @click="cerrar">Cerrar</b-button>
                </div>
            </div>
            <div v-if="!o_completas" class="permietro_historial_ordenes" >
                <b-container fluid>
                    <b-row>
                        <b-col sm="12" class="mt-3">
                            <table class="table table-sm table-striped tbmod">
                                <thead>
                                    <td style="width: 15%;">
                                        Orden No.
                                    </td>
                                    <td style="width: 55%">
                                        Descripción
                                    </td>
                                    <td style="width: 15%;text-align: center;">
                                        Fecha
                                    </td>
                                    <td style="width: 15%;text-align: center;">
                                        Etapa
                                    </td>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in orders" :key="index">
                                        <td>
                                            {{item.odt}}
                                        </td>
                                        <td>
                                            {{item.descripcion}}
                                        </td>
                                        <td style="text-align: center;">
                                            {{item.fecha_de_entrega}}
                                        </td>
                                        <td style="text-align: center;">
                                            {{item.etapa}}
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </b-col>
                    </b-row>
                </b-container>
            </div>
            <div v-if="o_completas" class="permietro_historial_ordenes">
                <b-container fluid>
                    <b-row>
                        <b-col sm="12" class="mt-3">
                            <table class="table table-sm table-striped tbmod">
                                <thead>
                                    <td style="width: 15%;">
                                        Orden No.
                                    </td>
                                    <td style="width: 55%">
                                        Descripción
                                    </td>
                                    <td style="width: 15%;text-align: center;">
                                        Fecha
                                    </td>
                                    <td style="width: 15%;text-align: center;">
                                        Etapa
                                    </td>
                                </thead>
                                <tbody>
                                    <tr v-for="(item, index) in ordenes_completas" :key="index">
                                        <td>
                                            {{item.odt}}
                                        </td>
                                        <td>
                                            {{item.descripcion}}
                                        </td>
                                        <td style="text-align: center;">
                                            {{item.fecha_de_entrega}}
                                        </td>
                                        <td style="text-align: center;">
                                            {{item.etapa}}
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

import { mapState } from 'vuex'

//tentativo, hay que mover a la funcion get_response
import { IP, PUERTO } from '@/config/parametros'
import axios from 'axios'

export default {

    name: 'Descripcion',
    props: ['idPac'],
    computed: {
        ...mapState(['odts'])
    },
    data() {
        return {
            orders: [],
            o_completas: false,
            ordenes_completas: []
        }
    },
    methods: {
        cerrar(){
            this.$emit('cerrarModal', false)
        },
        odtPorPaciente(){
            
            let ordenes = this.odts.filter(odt => odt.pid == this.idPac)
            this.orders = ordenes
        },
        async completas(){ // aqui
            this.o_completas = true

            let formulario = {
                id: this.idPac
            }

            let i = await axios.post(`http://${IP}:${PUERTO}/api/odt/eta`, formulario, this.$store.state.token)
            this.ordenes_completas = i.data
        },
        async transito(){
            this.o_completas = false
        }
    },
    mounted() {
        this.odtPorPaciente()
    },
}
</script>

<style>
    .contenedor-descripcion{
        width: 100%;
        height: 100vh;
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgba(0, 0, 0, .3);
        z-index: 999;
    }
        .cuerpo-descripcion{
            width: 700px;
            height: 500px;
            background-color: white;

        }
            .cabecera_decripcion{
                width: 100%;
                height: 40px;
                /* background-color: #8AC21D; */
                border-bottom: 1px solid #EAE8E6;
                display: flex;
                align-items: center;
                justify-content: space-between;
                padding-right: 10px;
                padding-left: 10px;
            }

            .permietro_historial_ordenes{
                width: 100%;
                height: calc(100% - 40px);
                overflow: auto;
            }

    .tbmod{
        font-size: 12px;
    }
</style>