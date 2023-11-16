<script >
  import { ref } from 'vue';
  import { evaluate } from 'mathjs';
  // import {create, all} from 'mathjs';

  export default{
    data(){
      return {
        result: 0,
        dec: 1,
        h: 0,
        n: 2, // Número de sub-intervalos
        a: 0, // Limite inferior
        b: 1, // Limite superior
        func: "x^2", // Função a ser integrada
        result: 0, // Resultado da integração
        fxi_values: [], // Valores da tabela
        tableValues: [], // Valores da tabela
        is_invalid: false,
        // math: create(all),
        error: null,
      }
    },
    methods: {
      clear() {
        this.n= 2;
        this.dec= 1;
        this.a= 0;
        this.b= 1;
        this.func= 'x^2';
        this.result= 0;
        this.tableValues= [];
        this.fxi_values = [];
        this.error = null;
      },
      validate() {
        
        this.error = null;
        this.fxi_values = [];
        this.tableValues= [];
        if(this.a >= this.b)
          this.error = "O valor de A não deve ser igual ou superior ao de B";
        else if(this.n <= 1 || this.n > 15)
          this.error = "O valor de N não deve ser inferior a 1 ou superior a 15";
        else{
          try {
            evaluate(this.func, { x: 2 });
            this.calculate();
          } catch (err) {
            this.error = "A função inserida é inválida " ;
          }
        }
      },
      calculate() {
        let val = (this.b - this.a) / this.n
        this.h =  val
        let base = this.a;
        // for()
        for (let i = 0; i <= this.n; i++) {
          const f = (i ==0 || i==this.n)? 'f'+i : '2*f'+i 
          let y= evaluate(this.func, { x: base })
          let c = (i ==0 || i==this.n)? y : y * 2
          const item_table = {
            xi: parseFloat(base).toFixed(this.dec),
            fxi: parseFloat(y).toFixed(this.dec),
            pond: c.toFixed(this.dec),
            form: f
          };
          const item = {
            xi: parseFloat(base),
            fxi: parseFloat(y),
            pond: c
          };
          this.tableValues.push(item_table);
          this.fxi_values.push(item);
          base += this.h;
          // alert(this.fxi_values.fxi)
        }
        let res = this.fxi_values.reduce((total, obj) => total + obj.pond, 0);
        // alert(res);
        // alert(this.h);
        res = res * this.h / 2 
        this.result = res.toFixed(this.dec);
      },  
      closeError(){
        this.error = null;
      },
      getResult(){
        return this.result.toFixed(this.dec)
      },
      getFuc(){
        return parseFloat(this.func)
      }

    }

  }
</script>

<template>
  <div class="container py-4">


    <div class="card shadow  text-white bg-dark " >
      <div class="card-header bg-">
        <h3 class="text-center text-warning">
          Integração pelo método de trapézio
        </h3>
      </div>
      <div class="card-body">

        <p class="card-text"><i>Informe os dados para a integração.</i> </p>
        <form action="">
          <div v-if="error != null" class="row">
            <div class="">
              <div class="alert alert-warning alert-dismissible fade show text-center" role="alert">
                <strong>Ohps!</strong> {{ error }}
                <button type="button" @click.prevent="closeError" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
              </div>
            </div>
          </div>
          <div class="row">
            
            <div class="col-12 col-md-4">
              <div class="form-group">
                <label for="">N (nr. de subintervalos)</label>
                <select v-model="n" class="form-select form-select-sm" aria-label="Default select example">
                  <option seleted value="2">2 SubIntervalos</option>
                  <option v-for="i= 3 in 10" :value="i +2" > {{ i + 2}} SubIntervalos</option>
                </select>
              </div>
            </div>
            <div class="col-12 col-md-4">
              <div class="form-group">
                <label for="">a (limite inferior)</label>
                <input v-model="a" type="number" class="form-control form-control-sm" placeholder="Insira o valor Limite Inferior">
              </div>
            </div>
            <div class="col">
              <div class="form-group">
                <label for="">b (limite superior)</label>
                <input v-model="b" type="number" class="form-control form-control-sm" placeholder="Last Superior">
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col-12 col-md-9">
              <div class="form-group">
                <label for="">Função</label>
                <input v-model="func" type="text"  class="form-control form-control-sm">
              </div>
            </div>
            <div class="col-12 col-md-3">
              <div class="form-group">
                <label for="">Casas decimais</label>
                <select v-model="dec" class="form-select form-select-sm" aria-label="Default select example">
                  <option seleted value="1">1 Casa</option>
                  <option v-for="i=3 in 4" :value="i+1"> {{ i + 1}} Casas</option>
                </select>
              </div>
            </div>
          </div>
          <button @click.prevent="validate" class="btn btn-success mt-2">Calcular</button>
        </form>

        <div v-if="result != 0" class="row py-3 rounded">
          <h4 class="text-info">H= {{ h }} </h4>
          <div class="col">
            <table class="table table-striped table-secondary rounded">
              <thead>
                <tr>
                  <th scope="col">i</th>
                  <th scope="col"><i>X<sub>i</sub></i></th>
                  <th scope="col"><i>f(X<sub>i</sub>)</i></th>
                  <th scope="col"><i>Fi</i></th>
                  <th scope="col">Form.</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(value, key) in tableValues">
                  <th scope="row">{{ key }}</th>
                  <td>{{ value.xi }}</td>
                  <td>{{ value.fxi }}</td>
                  <td>{{ value.pond }}</td>
                  <td>{{ value.form }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <h5  v-if="result != 0">Área Total: <span class="text-warning mt--5"> {{ result }} </span> </h5>
        <button  v-if="result != 0" @click="clear" class="btn btn-info btn-sm">Limpar</button>
      </div>
      <div class="bg-success pt-2">
        <i class="text-center text-light mt-4 "><h5> Edson Da Cruz & Paulo Novela</h5></i>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
