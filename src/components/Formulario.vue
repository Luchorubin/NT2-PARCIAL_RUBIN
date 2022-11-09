<template>

  <section class="src-components-formulario">
    <div class="jumbotron">
      <h2>Ingreso de gastos</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" v-model.trim="formData.nombre" required name="nombre" autocomplete="off" :minlength="minLengthString" :maxlength="maxLengthString" no-espacios class="form-control"/>
    
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo requiere como mínimo {{minLengthString}} caracteres.
            </div>
           <div slot="maxlength" class="alert alert-danger mt-1">
              Este campo admite como máximo {{maxLengthString}} caracteres.
            </div>
            <div slot="no-espacios" class="alert alert-danger mt-1">
              Este campo no admite espacios intermedios.
            </div>
          </field-messages>
        </validate>
        <br>

        <validate tag="div">
          <label for="descripcion">Descripción</label>
          <input type="text" id="descripcion" v-model.trim="formData.descripcion" required name="descripcion" autocomplete="off"  class="form-control" />
    
          <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>

        <validate tag="div">
          <label for="importe">Importe</label>
          <input type="text" id="importe" v-model.number="formData.importe" required name="importe" autocomplete="off" class="form-control" />
    
          <field-messages name="importe" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>

        <button class="btn btn-danger my-2" :disabled="formstate.$invalid" type="submit">ENVIAR</button>
      </vue-form>
    </div>
    <div class="jumbotron" id="jumbo2">
      <br>
      <label for="presupuesto">Ingrese su presupuesto</label>
      <br>
      <input type="number" v-model.number="presupuesto" name="presupuesto" id="presupesto" :style="presupuesto < gastoTotal() ? { 'color': 'red'} : ''"> 
      <br>
      <p style="color:red;">Si su presupuesto es menor al gasto total, aparecera en <b>ROJO</b></p>
      
      <br>
      <hr>
      <h2>Detalle de Gastos</h2>
      <br>

      <div v-if="gastos.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Nombre</th>
            <th>Descripcion</th>
            <th>Importe</th>
            <th>Fecha</th>
            
          </tr>
          <tr v-for="(gasto,index) in gastos" :key="index">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.descripcion }}</td>
            <td>${{ gasto.importe}}</td>
            <td>{{ gasto.fecha }}</td>
          </tr>
          <tr>
            <td><b>GASTO TOTAL:</b></td>
            <td :style="{color: analizarGastos(gastoTotal()).color}">{{gastoTotal()}}</td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>
    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate: {},
        formData : this.getInitialData(),
        gastos : [],
        minLengthString: 3,
        maxLengthString: 15,
        presupuesto: null,
      }
    },
    methods: {
      getInitialData(){
        return {
          nombre : '',
          descripcion: '',
          importe: null,
        }
      },
      enviar() {
        let gasto = {...this.formData}
        gasto.fecha = new Date().toLocaleString()

        console.log(gasto)
        this.gastos.push(gasto)

        this.formData = this.getInitialData()
        this.formstate._reset()
      },
      analizarGastos(gasto) {
        let color = ''
        if (gasto < 1000) {
          color = "green"
        }
        else if (gasto >= 1000 && gasto <= 5000) {
          color = "magenta"
        }
        else{
          color = 'orange'
        }
        return {
          color
        }
      },
      gastoTotal(){
        let gastoTot= 0
        this.gastos.forEach(a => {
          gastoTot += parseInt(a.importe, 10)
        });
       return gastoTot
      },
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }
  .jumbotron {
    background-color: yellow;
    color: black
  }

  #jumbo2{
    background-color: blue;
    color: white;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>
