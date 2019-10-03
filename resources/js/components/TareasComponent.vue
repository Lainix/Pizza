<template>
  <div>
    
    <form @submit.prevent="agregar" >
      <h3>Agregar Pedido ʕ•́ᴥ•̀ʔっ</h3>
      <input type="text" class="form-control mb-2" 
        placeholder="Nombre" v-model="nota.nombre">
        <input type="text" class="form-control mb-2" 
        placeholder="Numero de Pedido" v-model="nota.numero">
      <input type="text" class="form-control mb-2" 
        placeholder="Tamaño" v-model="nota.descripcion">
      <input type="text" class="form-control mb-2" 
        placeholder="Precio" v-model="nota.precio"> 
      <input type="text" class="form-control mb-2" 
        placeholder="Total" v-model="nota.total"> 
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
          <p>{{item.numero}}</p>
          <p>{{item.descripcion}}</p>            
          <p>{{item.precio}}</p>
          <p>{{item.total}}</p>    
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
      nota: {nombre: '', numero: '', descripcion: '', precio: '', total: ''},
    }
  },
  created(){
    axios.get('/notas').then(res=>{
      this.notas = res.data;
    })
  },
  methods:{
    agregar(){
      if(this.nota.nombre.trim() === '' || this.nota.numero.trim() === '' || this.nota.descripcion.trim() === '' || this.nota.precio.trim() === '' || this.nota.total.trim() === ''){
        alert('Debes completar todos los campos antes de guardar');
        return;
      }
      console.log(this.nota.nombre , this.nota.numero , this.nota.descripcion , this.nota.precio , this.nota.total);
        const params = {
        nombre: this.nota.nombre,
        numero: this.nota.numero,
        descripcion: this.nota.descripcion,
        precio: this.nota.precio,
        total: this.nota.total
    }   
    this.nota.nombre = '';
    this.nota.numero = '';
    this.nota.descripcion = '';
    this.nota.precio = '';
    this.nota.total = '';
      
      axios.post('/notas', params)
        .then((res) =>{
          this.notas.push(res.data);
        })

  }
 }
}
</script>