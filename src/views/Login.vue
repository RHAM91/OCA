<template>
    <div class="cuerpo">
        <div class="cuadro1">
            <div class="cuadrologo">
                <div class="logotipo">
                    <img src="@/assets/ortopedia-logo.png" alt="" style="width: 100%;">
                </div>
            </div>
            <footer>
                Powered by SIA v{{version}}
            </footer>
        </div>
        <div class="cuadro2">
            <div class="cuadrologin">
                <b-container fluid>
                    <form @submit.prevent="login">
                        <b-row>
                            <b-col sm="12">
                                <h1 class="tit">Iniciar sesión</h1>
                            </b-col>
                            <b-col sm="12" class="mt-4">
                                
                                <b-form-input type="text" size="sm" v-model="usuario" placeholder="Usuario" required=""></b-form-input>
                            </b-col>

                            <b-col sm="12" class="mt-2">
                                <b-form-input type="password" size="sm" v-model="password" placeholder="Contraseña" required=""></b-form-input>
                            </b-col>
                            <b-col sm="12" class="mt-3">
                                
                                <b-button type="submit" size="small" variant="dark" block pill>Entrar</b-button>
                            </b-col>
                        </b-row>
                    </form>
                </b-container>
                
            </div>
            
        </div>
    </div>
</template>

<script>

import { ipcRenderer } from 'electron'
window.ipcRenderer = ipcRenderer

import axios from 'axios'
import { mapState, mapActions, mapMutations } from 'vuex'
import { minix } from '../components/functions/alertas'
import { IP, PUERTO } from '../config/parametros'

export default {
    data(){
        return{
            usuario: '',
            password: '',
            version: ''
        }
    },
    computed:{
        ...mapState(['token'])
    },
    methods:{
        login: async function(){

            let formulario = {
                username: this.usuario,
                password: this.password
            }

            try {
                
                let data = await axios.post(`http://${IP}:${PUERTO}/api/auth/login`, formulario)
    
                if(data.data.message == 'PASSWORD INCORRECTO'){
    
                    minix({icon: 'error', mensaje: 'Contraseña incorrecta', tiempo: 3000})
    
                }else if(data.data.message == 'EL USUARIO NO EXISTE'){
    
                    minix({icon: 'error', mensaje: 'Usuario no existe', tiempo: 3000})
    
                }else{
                    this.get_token(data.data.token)
                    this.set_t2(data.data.token)

                    this.$router.replace('Main')
    
                }

            } catch (e) {
                minix({icon: 'error', mensaje: e.response.data.message, tiempo: 3000})
            }

        
            
        },
        getVersion(){
            ipcRenderer.send('app_version');
            ipcRenderer.on('app_version', (event, arg) => {
                ipcRenderer.removeAllListeners('app_version');
                this.version = arg.version
            });
        },
        ...mapActions(['get_token']),
        ...mapMutations(['set_t2'])
   
    },
    mounted() {
        this.getVersion()
    },
}
</script>

<style scoped>
    .cuerpo{
        width: 100%;
        height: 100vh;
        display: flex;

    }
        .cuadro1{
            width: 50%;
            height: 100%;
            background-color: rgb(255, 255, 255);
            position: relative;
        }
        .cuadro2{
            width: 50%;
            height: 100%;
            background-color: rgb(234, 121, 59);
            display: flex;
            justify-content: center;
            align-items: center;
        }
            .cuadrologin{
                width: 300px;
                height: auto;
            }
                .tit{
                    text-align: center;
                    color: white;
                }

        footer{
            width: 100%;
            height: auto;
            position: absolute;
            bottom: 5px;
            left: 0;
            text-align: center;
            color: rgb(199, 206, 212)
        }

        .cuadrologo{
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            user-select: none;

        }
            .logotipo{
                width: 300px;
                height: 300px;
            }
</style>