<template>
    <q-page>
      <div>
        <h2 class="text-center q-py-lg">Informações do Usuário</h2>
        <div class="q-mt-md q-pb-md config-wrapper w100 column justify-center items-center">
          <q-input filled readonly v-model="userData.tipoConta" label="Tipo de Conta" style="font-size: 16px" class="w20rem text-h6 low-opacity q-mb-md" />
          <q-input filled readonly v-model="userData.login" label="Login" style="font-size: 16px" class="w20rem text-h6 low-opacity q-mb-md" />
          <q-input filled readonly v-model="userData.email" style="font-size: 16px" label="Email" class="w20rem text-h6 low-opacity q-mb-md" />
          <q-input filled readonly v-model="userData.nome" label="Nome Completo" style="font-size: 16px" class="w20rem text-h6 low-opacity q-mb-md" />
          <q-input filled readonly v-model="userData.telefone" style="font-size: 16px" label="Telefone" mask="(##) ##### - ####" class="w20rem text-h6 low-opacity q-mb-md" />
          <q-input filled readonly v-model="userData.imagem" style="font-size: 16px" label="Link Imagem Perfil" class="w20rem text-h6 low-opacity q-mb-md" />
        </div>
        <div>
          <h2 class="text-center q-py-lg">Editar informações</h2>
          <div class="q-mt-md q-pb-md config-wrapper w100 column justify-center items-center">
            <q-input v-model="editedData.nome" outlined label="Nome Completo" style="font-size: 16px" class="w20rem text-h6 low-opacity q-mb-md" />
            <q-input v-model="editedData.telefone" style="font-size: 16px" label="Telefone" mask="(##) ##### - ####" class="w20rem text-h6 low-opacity q-mb-md" />
            <q-input v-model="editedData.novaImg" outlined style="font-size: 16px" label="Link Imagem Perfil" class="w20rem text-h6 q-mb-md" />
            <q-btn class="q-mt-md" color="primary" @click="enviar">Salvar Alterações</q-btn>
          </div>
        </div>
      </div>
    </q-page>
  </template>
  
  <script>
  export default {
    data() {
      return {
        editedData: {
          nome: '',
          telefone: '',
          novaImg: '',
        },
        userData: {
          tipoConta: 'Aluno',
          login: 'john_doe',
          email: 'john@example.com',
          nome: 'John Doe',
          telefone: '1234567890',
          imagem: 'https://example.com/user-image.jpg',
        },
      };
    },
    methods: {
      enviar() {
        // Simular uma chamada de API para salvar as alterações
        // Normalmente, você usaria um serviço para isso
        // Aqui, estamos apenas copiando os dados editados para os dados do usuário
        this.userData.telefone = this.editedData.telefone || this.userData.telefone;
        this.userData.imagem = this.editedData.novaImg || this.userData.imagem;
        this.userData.nome = this.editedData.nome || this.userData.nome;
  
        // Limpar os campos editados
        this.editedData = {
          nome: '',
          telefone: '',
          novaImg: '',
        };
  
        // Exibir mensagem de sucesso (você pode usar o Quasar Notify ou outra solução)
        this.$q.notify({
          type: 'positive',
          message: 'Alterações salvas com sucesso!',
          position: 'top',
        });
      },
    },
  };
  </script>
  
  <style scoped>
  .q-page {
    background: #a5dbff31;
    background: -webkit-linear-gradient(to top, #74ff7426, #ffffff, #55bbff58);
    background: linear-gradient(to top, #74ff7426, #ffffff, #55bbff58);
  }
  
  .config-wrapper {
    width: 10rem;
  }
  
  .w20rem {
    width: 20rem;
  }
  </style>