<template>
  <div>
    
    <form @submit.prevent="agregar" >
      <h3>Agregar Pedido ʕ•́ᴥ•̀ʔっ</h3>
      <input type="text" class="form-control mb-2" 
        placeholder="Nombre del pedido" v-model="nota.nombre">
      <input type="text" class="form-control mb-2" 
        placeholder="Ingredientes" v-model="nota.descripcion">
      <button class="btn btn-primary" type="submit">Agregar</button>
    </form>
    <hr>
    <h3>Lista de Pedidos:</h3>
    <ul class="list-group">
         <li class="list-group-item" 
            v-for="(item, index) in notas" :key="index" >
          <span class="badge badge-primary float-right"> 
            {{item.updated_at}}
          </span>
          <p>{{item.nombre}}</p>
          <p>{{item.descripcion}}</p>      
        </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notas: [],
      modoEditar: false,
      nota: {nombre: '', descripcion: ''},
    }
  },
  created(){
    axios.get('/notas').then(res=>{
      this.notas = res.data;
    })
  },
  methods:{
    agregar(){
      if(this.nota.nombre.trim() === '' || this.nota.descripcion.trim() === ''){
        alert('Debes completar todos los campos antes de guardar');
        return;
      }
      console.log(this.nota.nombre , this.nota.descripcion);
        const params = {
        nombre: this.nota.nombre,
        descripcion: this.nota.descripcion
    }   
    this.nota.nombre = '';
    this.nota.descripcion = '';
      
      axios.post('/notas', params)
        .then((res) =>{
          this.notas.push(res.data);
        })

  }
 }
}
</script>