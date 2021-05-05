<template>
  <div
    class="home row final d-flex justify-content-start align-items-center p-md-5 p-3 m-0"
  >
    <div class="col-12">
      <p class="color-text f-600">Potência mínima necessária: {{ kw }}Kw</p>
      <h3 class="color-text">
        Veja alguns resultados que selecionamos para você
      </h3>
      <hr class="color-text bg-secondary" />
    </div>
    <div class="row mt-4 w-100" v-if="!!productsResult.length">
      <div
        class="col-md-4 col-xl-3 mb-3"
        v-for="product in productsResult"
        :key="product.ref"
      >
        <div class="card product border-0 bg-tertiary">
          <div
            class="card-body color-text d-flex flex-column justify-content-center"
          >
            <img
              :src="product.image"
              alt="produto foto"
              class="img-fluid mb-3 d-flex mx-auto w-75"
            />
            <h6>{{ product.name }}</h6>
            <div class="d-flex justify-content-between align-items-center">
              <span class="badge badge-primary bg-dark"
                >{{ product.kw }} Kw</span
              >
              <span class="badge badge-primary bg-dark"
                >Ref: {{ product.ref }}</span
              >
            </div>
          </div>
          <div class="card-footer border-0">
            <hr class="bg-secondary w-100" />
            <a
              :href="product.url"
              target="_blank"
              class="btn btn-tertiary d-flex align-items-center justify-content-center"
              >Visualizar</a
            >
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-5" v-else>
      <div class="col-12">
        <div class="card border-0 shadow-sm bg-tertiary">
          <div class="card-body color-text">
            <feather class="color-secondary" type="alert-triangle"></feather>
            <h5 class="pt-3">Precisa de um projeto personalizado?</h5>
            <p>
              A demanda para seu ambiente é maior do que a capacidade de um
              único equipamento. Entre em contato com nosso SAC para avaliarmos
              o seu projeto
            </p>
            <p class="d-flex justify-content-start align-items-start">
              <feather class="color-text f-12 pr-2" type="mail"></feather>
              <a class="color-primary f-800" href="mailto:sac@metavila.com.br"
                >sac@metavila.com.br</a
              >
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-4" v-show="!!productsResult.length">
      <div class="col-12 mt-5">
        <h3 class="color-text">
          <feather class="color-primary" type="alert-triangle"></feather>
          Informações importantes
        </h3>
      </div>
      <div class="col-md-5">
        <div class="card border-0 shadow-sm bg-primary tips mt-3">
          <div class="card-body text-white">
            <img src="@/assets/img/windows.svg" alt="Janela" class="img-fluid pb-3" width="50">
            <h5>1. Fator de Isolamento</h5>
            <hr class="bg-secondary" />
            <p class="small lh-2">
              O isolamento do ambiente pode interferir na capacidade de
              aquecimento de seu produto de dupla combustão. Instalações
              realizadas próximas a portas janelas ou ambientes que contenham
              muito vidro, podem diminuir a capacidade de aquecimento.
            </p>
          </div>
        </div>
      </div>
      <div class="col-md-5">
        <div class="card border-0 shadow-sm bg-primary tips mt-3">
          <div class="card-body text-white">
            <img src="@/assets/img/compass.svg" alt="Rosa dos Ventos" class="img-fluid pb-3" width="50">
            <h5>2. Outros Fatores</h5>
            <hr class="bg-secondary" />
            <p class="small lh-2">
              Outros fatores como a orientação (norte, sul, leste, oeste) solar
              do ambiente onde o produto estiver instalado e a temperatura
              externa também são variáveis no potencial calorífico do seu
              calefator, inserto ou fogão de alto rendimento.
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="row" style="height: 100px"></div>
  </div>
</template>

<script>
import produtos from "../../server/products.json";
export default {
  name: "Steps",
  data() {
    return {
      productsResult: [],
      loading: false,
      calc_meters: 0,
      kw: 0,
      list: []
    };
  },
  methods: {
    filterProducts(product_list) {
      const list = product_list.filter((e) => {
        let val = parseInt(e.kw);
        let max = parseFloat(this.kw) + 3;
        return val > this.kw && val < max;
      });
      this.productsResult.push(...list);
      this.productsResult.sort(function (a, b) {
        return a.kw - b.kw;
      });
    },
  },
  created() {
    const data = {
      alt: sessionStorage.altura,
      lar: sessionStorage.largura,
      comp: sessionStorage.comprimento,
    };
    this.$router.push({ query: data });
    const { liv } = produtos;
    const { comp, lar, alt } = this.$route.query;
    this.calc_meters = comp * lar * alt;
    this.kw = (this.calc_meters / 26).toFixed(1);
    this.filterProducts(liv);
  },
};
</script>

<style lang="scss" scoped>
.tips {
  min-height: 250px;
  .lh-2 {
    line-height: 1.4rem;
  }
}
.product {
  .card-body {
    min-height: 330px;
  }
}
</style>
