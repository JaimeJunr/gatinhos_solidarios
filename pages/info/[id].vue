<template>
  <div class="container mt-4">
    <div v-if="cat" class="card mb-3">
      <img :src="cat.imagem" class="card-img-top" alt="Gatinho" />
      <div class="card-body">
        <h5 class="card-title">{{ cat.nome }}</h5>
        <p class="card-text">{{ cat.descricao }}</p>

        <h6 class="mt-3">Ajude a Encontrar {{ cat.nome }}</h6>
        <p>
          Se você tiver alguma informação sobre o paradeiro de {{ cat.nome }},
          entre em contato conosco.
        </p>

        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="nome">Seu Nome:</label>
            <input
              type="text"
              class="form-control"
              id="nome"
              v-model="form.nome"
              required
            />
          </div>
          <div class="form-group">
            <label for="email">Seu E-mail:</label>
            <input
              type="email"
              class="form-control"
              id="email"
              v-model="form.email"
              required
            />
          </div>
          <div class="form-group">
            <label for="mensagem">Mensagem:</label>
            <textarea
              class="form-control"
              id="mensagem"
              v-model="form.mensagem"
              rows="3"
              required
            ></textarea>
          </div>
          <button type="submit" class="btn btn-primary mt-2 mb-1">Enviar</button>
        </form>
        <div class="social-sharing">
                  <a
                    :href="getFacebookShareLink(cat)"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="40"
                      height="40"
                      fill="currentColor"
                      class="bi bi-facebook"
                      viewBox="0 0 24 16"
                    >
                      <path
                        d="M16 8.049c0-4.446-3.582-8.05-8-8.05C3.58 0-.002 3.603-.002 8.05c0 4.017 2.926 7.347 6.75 7.951v-5.625h-2.03V8.05H6.75V6.275c0-2.017 1.195-3.131 3.022-3.131.876 0 1.791.157 1.791.157v1.98h-1.009c-.993 0-1.303.621-1.303 1.258v1.51h2.218l-.354 2.326H9.25V16c3.824-.604 6.75-3.934 6.75-7.951"
                      />
                    </svg>
                  </a>
                  <a
                    :href="getTwitterShareLink(cat)"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="40"
                      height="40"
                      fill="currentColor"
                      class="bi bi-twitter"
                      viewBox="0 0 24 16"
                    >
                      <path
                        d="M5.026 15c6.038 0 9.341-5.003 9.341-9.334q.002-.211-.006-.422A6.7 6.7 0 0 0 16 3.542a6.7 6.7 0 0 1-1.889.518 3.3 3.3 0 0 0 1.447-1.817 6.5 6.5 0 0 1-2.087.793A3.286 3.286 0 0 0 7.875 6.03a9.32 9.32 0 0 1-6.767-3.429 3.29 3.29 0 0 0 1.018 4.382A3.3 3.3 0 0 1 .64 6.575v.045a3.29 3.29 0 0 0 2.632 3.218 3.2 3.2 0 0 1-.865.115 3 3 0 0 1-.614-.057 3.28 3.28 0 0 0 3.067 2.277A6.6 6.6 0 0 1 .78 13.58a6 6 0 0 1-.78-.045A9.34 9.34 0 0 0 5.026 15"
                      />
                    </svg>
                  </a>
                </div>
      </div>
    </div>
    <div v-else>
      <p>Carregando...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

import gatinhos from "~/data/gatinhos";

const route = useRoute();
const cat = ref(null);

onMounted(() => {
  const catId = Number(route.params.id);
  if (!isNaN(catId)) {
    cat.value = gatinhos.find((gatinho) => gatinho.id === catId) || null;
  } else {
    console.error("Invalid catId:", route.params.id);
  }
});

const form = ref({
  nome: "",
  email: "",
  mensagem: "",
});

const handleSubmit = () => {
  console.log("Form data:", form.value);
  // Handle form submission (e.g., send the data to a server)
};

const getFacebookShareLink = (gatinho) => {
  // Substitua 'SEU_URL_DO_SITE' pelo URL do seu site
  return `https://www.facebook.com/sharer/sharer.php?u=localhost/info/${gatinho.id}`;
};

// Função para obter o link de compartilhamento do Twitter
const getTwitterShareLink = (gatinho) => {
  // Substitua 'SEU_URL_DO_SITE' pelo URL do seu site e 'SEU_HANDLE_DO_TWITTER' pelo seu handle do Twitter
  return `https://twitter.com/intent/tweet?url=SEU_URL_DO_SITE/info/${gatinho.id}&text=Conheça ${gatinho.nome} - Um gatinho adorável! @SEU_HANDLE_DO_TWITTER`;
};
</script>

<style scoped>
.card-img-top {
  height: 400px;
  object-fit: cover;
}
.container {
  padding: 1rem;
}
</style>
