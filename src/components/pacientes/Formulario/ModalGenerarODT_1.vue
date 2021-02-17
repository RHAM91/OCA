<template>
    <div class="cuerpo_generar_odt">
        <b-button type="button" size="sm" variant="warning" class="btn_x" @click="salir">Cerrar</b-button>
        <div class="cuadro_generar_odt">
            <GenerarODTxxx :remoto="true" :info_paciente="info" />
        </div>
    </div>
</template>

<script>

import GenerarODTxxx from '@/components/ordendetrabajo/NuevaODT/NuevoRegistro.vue'

export default {
    name: "GenerarODT__1",
    props: ['info'],
    components:{
        GenerarODTxxx
    },
    data() {
        return {
            info_paciente: ''
        }
    },
    methods: {
        salir(){
            this.$emit('cerrar_modal', false)
        },
    }
}
</script>

<style>
    .cuerpo_generar_odt{
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .3);
        position: fixed;
        top: 0;
        left: 0;
        z-index: 999;
        display: flex;
        justify-content: center;
        align-items: center;
        
    }
        .btn_x{
            position: fixed;
            top: 25px;
            right: 15px;
        }

        .cuadro_generar_odt{
            width: 800px;
            height: 725px;
            background-color: white;
            overflow: auto;
            padding-bottom: 10px;
        }
</style>