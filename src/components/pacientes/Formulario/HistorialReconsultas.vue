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
                                        <th style="text-align: center;width: 15%;">
                                            Código
                                        </th>
                                        <th style="text-align: center;width: 55%;">
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
                                    <tr>
                                        <td>

                                        </td>
                                        <td>

                                        </td>
                                        <td>

                                        </td>
                                        <td>
                                            
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
export default {
    name: "HistorialReconsultas",
    data() {
        return {
            
        }
    },
    methods: {
        salir(){
            this.$emit('cerrar_modal', false)
        }
    },
}
</script>

<style>

</style>