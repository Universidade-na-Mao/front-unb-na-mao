<template>
    <div id="login-wrapper" class="relative column justify-center items-center">
        <div class="background-image"></div>
        <div class="login-header">
            <div class="text-white text-h4 text-bold q-pa-md text-center" style="font-family:'Ubuntu'">UnB na Mão</div>
            <div style="width:100%;height:1px;background: rgba(184, 184, 184, 0.5)"></div>
        </div>
        <div class="login-body q-px-md q-pt-md">
            <q-select filled v-if="cadastrando" color="blue-9" :options="roleOptions" label="Tipo de Conta *" maxlength="40"
                class="bg-grey-2 q-mb-md text-white" v-model="role" />
            <q-input filled v-if="cadastrando" color="blue-9" dense label="Nome *" maxlength="40"
                class="bg-grey-2 q-mb-md text-white" v-model="nome" />
            <q-input filled color="blue-9" dense label="Login *" maxlength="9" class="bg-grey-2 text-white"
                v-model="login" />
            <q-input filled v-if="cadastrando" color="blue-9" dense label="Telefone *" mask="(##) #####-####"
                class="bg-grey-2 q-mt-md text-white" v-model="telefone" />
            <q-input filled v-if="cadastrando" color="blue-9" dense label="Email *" maxlength="35"
                class="bg-grey-2 text-white q-mt-md" v-model="email" />
            <q-input filled color="blue-9" dense :label="cadastrando ? 'Criar Senha *' : 'Senha *'" maxlength="15"
                :type="isPwd ? 'password' : 'text'" class="bg-grey-2 text-white q-mt-md" v-model="senha">
                <template v-slot:append>
                    <q-icon :name="isPwd ? 'visibility_off' : 'visibility'" class="cursor-pointer q-pr-sm"
                        @click="isPwd = !isPwd" />
                </template>
            </q-input>
            <div v-if="!cadastrando" class=" btns-wrapper w100 column justify-center q-mt-xs q-mb-md q-gutter-y-md">
                <q-btn icon-right="login" @click="logar" label="Entrar" color="positive" class="q-px-lg" />
                <q-btn icon-right="group_add" @click="toggleCadastrando" label="Cadastre-se" color="orange-7"
                    class="q-px-md" />
            </div>
            <div v-if="cadastrando" class=" btns-wrapper w100 column justify-center q-mt-xs q-mb-md q-gutter-y-md">
                <q-btn @click="cadastrar" icon-right="person_add" label="Finalizar Cadastro" color="positive"
                    class="q-px-lg" />
                <q-btn @click="toggleCadastrando" label="Voltar" color="grey-6" class="q-px-md" />
            </div>
        </div>
        <div class="top-div">
            <button @click="esqueciSenha" filled id="button" flat class="q-mt-sm bg-yellow-10 text-white q-pa-sm">
                Esqueci minha Senha
            </button>
            <div v-if="modalEsqueciSenha" class="modal">
                <div class="modal-esqueci-senha rounded-borders">
                    <div class="w100 row no-wrap justify-between items-center q-py-md">
                        <div class="text-h5 q-pl-md">Redefinir Senha</div>
                        <q-icon name="close" size="lg" class="cursor-pointer q-pr-sm close" color="red"
                            @click="esqueciSenha" />
                    </div>
                    <form v-if="!codigoEnviado" class="relative row no-wrap q-mb-md q-gutter-x-sm q-px-md  items-center">
                        <q-input maxlength="60" name="user_email" autocomplete="on" required filled v-model="email"
                            :readonly="codigoEnviado" dense class="bg-white w100" label="E-mail *" />
                        <q-btn @click="sendEmail()" id="codigo" dense class="bg-yellow-10 text-white">Enviar Código</q-btn>
                    </form>
                    <div v-if="codigoEnviado && !novaSenha"
                        class="q-px-md row no-wrap items-center justify-between q-mb-md">
                        <p class="no-margin w100">Digite o código que foi enviado em seu e-mail:</p>
                        <q-input label="Código" maxlength="4" mask="####" outlined v-model="campoCodigo" dense
                            class="bg-white " />
                    </div>
                    <div class="flex justify-center items-center">
                        <q-btn v-if="codigoEnviado && !novaSenha"
                            :disable="!campoCodigo || campoCodigo && campoCodigo.length < 4" @click="resetarSenha"
                            class="bg-orange-8 text-white" style="width: 200px; height: 50px;">Verificar Código</q-btn>
                    </div>
                    <div v-if="novaSenha" class="q-px-md column no-wrap q-mb-md">
                        <q-input label="Nova Senha" maxlength="20" v-model="senha" dense class="bg-white " />
                        <q-btn class="bg-green-5 text-white q-mt-md">Resetar a senha</q-btn>
                    </div>
                </div>
            </div>

        </div>


        <LoadingComponent v-if="loading" />
    </div>
</template>
<script lang="ts" setup>
import { useQuasar } from 'quasar';
import { api } from 'src/boot/axios';
import { ref } from 'vue';

import LoadingComponent from 'src/components/LoadingComponent.vue';
import { useRouter } from 'vue-router';
import emailjs from '@emailjs/browser';
import { index } from 'cheerio/lib/api/traversing';
import { isImportTypeAssertionContainer } from 'typescript';

const $q = useQuasar()
const loading = ref(false)

const isPwd = ref(true)
const login = ref('')
const senha = ref('')
const nome = ref('')
const email = ref('')
const telefone = ref('')
const role = ref('')
const roleOptions = ref(['Estudante', 'Professor', 'Atlética', 'Empresa Júnior', 'Empresa Externa'])

const modalEsqueciSenha = ref(false)

const cadastrando = ref(false)

const codigoEnviado = ref(false)
const campoCodigo = ref(null) as any
const codigoGerado = ref(null) as any

const novaSenha = ref(false)

const esqueciSenha = () => {
    modalEsqueciSenha.value = !modalEsqueciSenha.value
    codigoEnviado.value = false
    campoCodigo.value = null
    novaSenha.value = false
}

function sendEmail() {
    codigoGerado.value = generateFourDigitCode()
    const emailData = {
        user_email: email.value,
        message: codigoGerado.value,
    };
    enviarCodigo();
    emailjs.send('service_nbmlzbe', 'template_sl5yvnd', emailData, 'KD15sQJ-xFghabjnw')
        .then((result) => {
            console.log('SUCCESS!');
        })
        .catch((error) => {
            console.log('FAILED...');
        });
}

function generateFourDigitCode() {
    // Gera um número aleatório entre 0 e 9999
    const randomNumber = Math.floor(Math.random() * 10000);

    // Formata o número para ter sempre 4 dígitos, adicionando zeros à esquerda se necessário
    const codigoGerado = randomNumber.toString().padStart(4, '1');

    return Number(codigoGerado);
}


const enviarCodigo = () => {
    codigoEnviado.value = true
}

const resetarSenha = () => {
    if (campoCodigo.value == codigoGerado.value) {
        $q.notify({
            color: 'green-8',
            textColor: 'white',
            icon: 'check',
            message: 'Código Correto',
            position: 'top',
        });
        novaSenha.value = true
    } else {
        
        $q.notify({
            color: 'red-8',
            textColor: 'white',
            icon: 'warning',
            message: 'O código digitado está incorreto.',
            position: 'top',
            

        });
        setTimeout(() => {
            window.location.reload()
        }, 1000);
    }
}

const toggleCadastrando = () => {
    cadastrando.value = !cadastrando.value
}

function printNegative(msg: string) {
    $q.notify({
        message: msg,
        color: 'red-6',
        position: 'top',
        icon: 'report_problem'
    })
}

function validaCampos() {
    if (!email.value.includes('@') || !email.value.includes('.')) {
        printNegative('Email inválido')
        return false
    } else if (telefone.value.length != 15) {
        printNegative('Telefone inválido')
        return false
    } else if (senha.value.length < 6) {
        printNegative('Senha deve ter no mínimo 6 caracteres')
        return false
    } else {
        return true
    }
}

function limparCampos() {
    login.value = ''
    senha.value = ''
    nome.value = ''
    email.value = ''
    telefone.value = ''
    role.value = ''
}

const router = useRouter()

const logar = async () => {
    if (login.value && senha.value) {
        loading.value = true
        const loginObject = {
            matricula: login.value,
            senha: senha.value
        }
        await api.post('/login', loginObject)
            .then(response => {
                $q.notify({
                    message: response.data.message,
                    color: response.data.message.includes('não') ? 'red-6' : 'green-7',
                    position: 'top',
                    icon: response.data.message.includes('não') ? 'error' : 'check_circle'
                })
                loading.value = false
                if (response.status == 201) {
                    setTimeout(() => {
                        router.push('/app')
                    }, 1500)
                }
            })
            .catch(error => {
                $q.notify({
                    message: error.response.data.message,
                    color: 'red-6',
                    position: 'top',
                    icon: 'report_problem'
                })
                loading.value = false
            })
    } else {
        printNegative('Preencha todos os campos')
    }
}

const cadastrar = async () => {
    if (login.value && senha.value && nome.value && email.value && telefone.value && role.value && validaCampos()) {
        loading.value = true
        const data = {
            nome: nome.value.toLowerCase(),
            matricula: login.value,
            senha: senha.value,
            telefone: telefone.value,
            email: email.value,
            role: role.value
        }
        await api.post('/usuarios', data)
            .then(response => {
                $q.notify({
                    message: response.data.message,
                    color: 'green-6',
                    position: 'top',
                    icon: 'check_circle'
                })
                loading.value = false
                limparCampos()
                toggleCadastrando()
            })
            .catch(error => {
                $q.notify({
                    message: error.response.data.message,
                    color: 'red-6',
                    position: 'top',
                    icon: 'report_problem'
                })
                loading.value = false
            })
    } else {
        $q.notify({
            message: 'Verifique todos os campos obrigatórios',
            color: 'orange-8',
            position: 'top',
            icon: 'info'
        })
    }
}

</script>

<style scoped>
#login-wrapper {
    width: 100vw;
    height: 100vh;
    overflow: hidden;
}

.background-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('https://images.unsplash.com/photo-1651510622356-c6bd9fc204ba?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D');
    background-size: cover;
    animation: zoom 60s linear infinite;
}

@keyframes zoom {
    0% {
        transform: scale(1);
    }

    50% {
        transform: scale(1.2);
    }

    100% {
        transform: scale(1);
    }
}

.login-header,
.login-body {
    width: 30rem;
    z-index: 99;
}

.login-header {
    border-top-right-radius: 8px;
    border-top-left-radius: 8px;
}

.login-body {
    border-bottom-right-radius: 8px;
    border-bottom-left-radius: 8px;
}

.login-body,
.login-header {
    filter: drop-shadow(0px 0px 10px rgba(0, 0, 0, 0.25));
    background-color: rgba(0, 0, 0, 0.418);
    backdrop-filter: blur(10px);
}

.q-input,
.q-select {
    font-family: 'Ubuntu';
    border-radius: 4px;
}

@media (max-width:520px) {

    .login-body,
    .login-header {
        width: 95%;
    }
}

@media (max-width: 800px) {
    .background-image {
        animation: none !important;
    }
}

.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    backdrop-filter: blur(5px);
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 100;
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-esqueci-senha {
    background-color: #fafafa;
    width: 35rem;
    
}



.top-div {
    position: relative;
    z-index: 9999;
}

</style>
