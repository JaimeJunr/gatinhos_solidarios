<style scoped>
.cats {
  overflow-x: scroll;
  flex-wrap: nowrap;
}

.card-img-top {
  height: 300px;
}

.card-text {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}
</style>

<template>
  <div>
    <header>
      <nav class="navbar navbar-dark bg-dark">
        <div class="container">
          <a class="navbar-brand" href="#">Gatinhos Solidários</a>
        </div>
      </nav>
    </header>

    <main>
      <section class="container mt-4">
        <div class="row">
          <div class="col-md-6">
            <h2>Sobre</h2>
            <p>
              A enchente recente no Rio Grande do Sul resultou no
              desaparecimento de muitos gatinhos adoráveis. Junte-se a nós para
              ajudar a encontrar e cuidar desses gatinhos.
            </p>
          </div>
          <div class="col-md-6">
            <img
              src="/assets/gatinho-desaparecido.png"
              alt="Gatinho desaparecido"
              class="img-fluid"
            />
          </div>
        </div>
      </section>

      <section class="bg-light py-4">
        <div class="container">
          <h2>Como Você Pode Ajudar?</h2>
          <p>
            Existem várias maneiras de ajudar gatos desaparecidos após uma
            enchente:
          </p>
          <ul>
            <li>Voluntarie-se em abrigos de animais.</li>
            <li>Faça doações para organizações de resgate de animais.</li>
            <li>
              Compartilhe informações sobre gatos desaparecidos em suas redes
              sociais.
            </li>
          </ul>
        </div>
      </section>

      <section class="container mt-4">
        <h2>Galeria de Gatinhos</h2>

        <div class="row">
          <div class="col-md-3 mb-3">
            <label for="sort-by">Ordenar por:</label>
            <select v-model="sortBy" id="sort-by" class="form-control">
              <option value="name-asc">Nome (A-Z)</option>
              <option value="name-desc">Nome (Z-A)</option>
              <option value="age-asc">Idade (Mais Novo)</option>
              <option value="age-desc">Idade (Mais Velho)</option>
            </select>
          </div>
        </div>

        <div class="row cats">
          <div
            v-for="gatinho in filteredSortedGatinhos"
            :key="gatinho.id"
            class="col-md-4 mb-3"
          >
            <div class="card">
              <img :src="gatinho.imagem" class="card-img-top" alt="..." />
              <div class="card-body">
                <h5 class="card-title">{{ gatinho.nome }}</h5>
                <p class="card-text">{{ gatinho.descricao }}</p>
                <NuxtLink
                  :to="{ name: 'info-id', params: { id: gatinho.id } }"
                  class="btn btn-primary"
                  >Ver Detalhes</NuxtLink
                >
                <div class="social-sharing">
                  <a
                    :href="getFacebookShareLink(gatinho)"
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
                    :href="getTwitterShareLink(gatinho)"
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
          </div>
        </div>
      </section>
    </main>

    <footer class="mt-4 bg-dark text-white py-3">
      <div class="container text-center">
        &copy; 2024 Gatinhos Solidários - Todos os direitos reservados.
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import "bootstrap/dist/css/bootstrap.min.css";

import gatinhos from "~/data/gatinhos";

let sortBy = ref("name-asc");

const filteredSortedGatinhos = computed(() => {
  let filteredGatinhos = gatinhos;

  // Aplicar critério de ordenação
  if (sortBy.value === "name-asc") {
    filteredGatinhos.sort((a, b) => a.nome.localeCompare(b.nome));
  } else if (sortBy.value === "name-desc") {
    filteredGatinhos.sort((a, b) => b.nome.localeCompare(a.nome));
  } else if (sortBy.value === "age-asc") {
    filteredGatinhos.sort((a, b) => a.idade - b.idade);
  } else if (sortBy.value === "age-desc") {
    filteredGatinhos.sort((a, b) => b.idade - a.idade);
  }

  return filteredGatinhos;
});

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
