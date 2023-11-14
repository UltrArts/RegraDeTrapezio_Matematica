<script setup>

</script>

<template>
  <div class="container py-2">


    <div class="card  text-white bg-dark " >
      <div class="card-header">
        <h3 class="text-center text-warning">
          Integração pelo método de trapézio
        </h3>
      </div>
      <div class="card-body">

        <p class="card-text"><i>Informe os dados para a integração.</i> </p>
        <form action="">
          <div class="row">
            <div class="col">
              <div class="form-group">
                <label for="">N (número de sub-intervalos)</label>
                <input type="number" min="2" class="form-control form-control-sm" placeholder="Insira o número de sub-intervalos">
              </div>
            </div>
            <div class="col">
              <div class="form-group">
                <label for="">a (limite inferior)</label>
                <input type="number" class="form-control form-control-sm" placeholder="Insira o valor Limite Inferior">
              </div>
            </div>
            <div class="col">
              <div class="form-group">
                <label for="">b (limite superior)</label>
                <input type="number" class="form-control form-control-sm" placeholder="Last Superior">
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col">
              <div class="form-group">
                <label for="">Função</label>
                <input type="text"  class="form-control form-control-sm">
              </div>
            </div>
          </div>
          <button class="btn btn-success mt-2">Calcular</button>
        </form>

        <div class="row py-3 rounded">
          <div class="col-4 ">
            <table class="table table-striped table-secondary rounded">
              <thead>
                <tr>
                  <th scope="col">i</th>
                  <th scope="col"><i>x<sub>i</sub></i></th>
                  <th scope="col"><i>f(x<sub>i</sub>)</i></th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <th scope="row"></th>
                  <td></td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="col-8">
            <table class="table table-striped table-secondary rounded">
              <thead>
                <tr>
                  <th scope="col">i</th>
                  <th scope="col">Área do Trapézio = h/2<i>f(x<sub>i</sub>)</i></th>
                  <th scope="col">Ponderação = Área do trapézio/Área total</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <th scope="row"></th>
                  <td></td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <h5>Área Total: <span class="text-warning"> Variavel do resultado</span> </h5>
        <button class="btn btn-info btn-sm">Limpar</button>
      </div>
    </div>


  </div>
</template>

<style scoped>

</style>
