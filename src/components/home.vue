<template>
 <v-app>
    <v-card width="500" class="mx-auto mt-10">
        <v-card-title>
            <h1 class='display-1'> Crear Paciente</h1>
        </v-card-title>

        <v-card-text>
            <v-form>
                <v-text-field label='Nombre' v-model="nombre" prepend-icon="mdi-account-circle"/> 
                <v-text-field label= "Edad" v-model="edad" prepend-icon="mdi-lock"/>
                <v-text-field label= "DNI"  v-model="dni"  prepend-icon="mdi-baguette" :type="showDNI ? 'text' : 'password'" @click:append="showDNI = !showDNI" append-icon="mdi-hotel"/>
                <v-text-field label= "Obra Social" v-model="obrasocial" prepend-icon="mdi-battery-30" />
            </v-form>
        </v-card-text>
        <v-card-actions>
            <v-btn color="success" @click="crearUsuario"> Crear </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="info"> Cancelar </v-btn>
        </v-card-actions>
 
    </v-card> 
    <v-data-table
        :headers="headers"
        :items="pacientes"
        :items-per-page="10"
        class="elevation-1"
    ></v-data-table>       
 </v-app>
</template>

<script>
import axios from 'axios';
export default {
  name: 'home',
  data: () =>({
      showDNI : true,
      nombre : '',
      edad : '',
      dni : '',
      obrasocial : '',
      pacientes : [],
      headers: [
        {
          text: 'Nombre',
          align: 'left',
          sortable: false,
          value: 'nombre',
        },
        { text: 'edad', value: 'edad' },
        { text: 'dni', value: 'dni' },
        { text: 'obrasocial', value: 'obrasocial' },
      ],
  }),
  methods:{
      crearUsuario : function() {
        this.pacientes.push({
            nombre : this.nombre,
            edad : this.edad,
            dni : this.dni,
            obrasocial : this.obrasocial
        })
    }
  },
  mounted() {
    axios.post(`http://localhost:8083/graphql`,{
        query: ` {
                 allPaciente{
                     nombre
                     edad
                     dni
                     obrasocial
                 }
                }`
    })
    .then(response => {
      // JSON responses are automatically parsed.
      this.pacientes = response.data.data.allPaciente


    })    .catch(e => {
             /* eslint-disable no-console */
             console.log(e)
        /* eslint-enable no-console */
    })
  }
  
}
</script>
