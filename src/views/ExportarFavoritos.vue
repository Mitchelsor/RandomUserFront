<template>
  <div>
    <v-row class="mt-7">
      <v-container class="borde">
        <v-col>
          <table>
            <tr>
              <th style="width: 80px">Género</th>
              <th style="width: 160px">Nombre</th>
              <th style="width: 240px">Email</th>
              <th style="width: 120px">Nacionalidad</th>
              <th style="width: 150px">Fecha Nacimiento</th>
              <th style="width: 150px">Fecha Registro</th>
             
            </tr>
            <tr v-for="(user, index) in $store.state.favoritos" :key="index">
              <td>{{ user.gender | genero }}</td>
              <td>
                <span>{{ user.name.first }} {{ user.name.last }}</span>
              </td>
              <td>
                <span
                  style="width: 10px !important; margin-right: 20px !important"
                  >{{ user.email }}</span
                >
              </td>
              <td><span style="margin-left: 30px"></span>{{ user.nat }}</td>
              <td>{{ user.dob.date | date }}</td>
              <td>{{ user.registered.date | date }}</td>
            
            </tr>
          </table>
        </v-col>
        <v-row>
             <v-col md="5" class="ml-3">
         
            <v-btn color="pink" justify="center" align="center" dark @click="exportTableToCSV('members.csv')"
          >Exportar <v-icon class="ml-2">mdi-export</v-icon></v-btn
        >
           <v-btn color="blue" text class="ml-4" @click="crearLista = true" dark>Crear lista de favoritos </v-btn>  
          

        </v-col>
        <v-col md="3" v-if="crearLista">
               <v-text-field class="ml-n16" v-model="nickname"  placeholder="Ingrese el nickname de la lista" outlined dense hide-details></v-text-field>

        </v-col>
        <v-col md="1" class="ml-n5" v-if="crearLista">
          <v-btn @click="guardarLista" class="ml-n14" color="green" dark ><v-icon>mdi-content-save</v-icon></v-btn>
        </v-col>
        </v-row>
     
        
      </v-container>
    </v-row>
    <v-row class="mt-2">
      <v-spacer class="mt-15"></v-spacer>
    </v-row>
  </div>
</template>

<script>


export default {
  name: "PruebaHome",

  data: () => ({
    users: [],
    dataApi: [],
    nickname: '',
    crearLista: false,
  }),
  created(){
      // this.axios.get('http://localhost:3000/api/favoritos').then( (res) => {
      //       console.log(res);
          
      //   });
  },
  mounted() {
    this.users = this.$store.state.favoritos;
  },

  methods: {
    exportarUsuario(user) {
      this.$store.state.usuario = user;
      this.$router.push("/perfil");
    },

    activarCampoCrearLista(){
        this.crearLista = true;
    },

    cargarUsuarios() {
      this.users = this.dataApi;
    },



    guardarLista(){
        
        let data = {
          nickname: this.nickname,
          favoritos: this.$store.state.favoritos,
        }
      
        this.axios.post('http://localhost:3000/api/favoritos', data ).then( (res) => {
            console.log(res);
        });

        
      
    },

    // Funciones para exportar a CSV
    exportTableToCSV(filename) {
      var csv = [];
      var rows = document.querySelectorAll("table tr");

      for (var i = 0; i < rows.length; i++) {
        var row = [],
          cols = rows[i].querySelectorAll("td, th");

        for (var j = 0; j < cols.length; j++) row.push(cols[j].innerText);

        csv.push(row.join(","));
      }

      // Download CSV file
      this.downloadCSV(csv.join("n"), filename);
    },
    downloadCSV(csv, filename) {
      var csvFile;
      var downloadLink;

      // CSV file
      csvFile = new Blob([csv], { type: "text/csv" });

      // Download link
      downloadLink = document.createElement("a");

      // File name
      downloadLink.download = filename;

      // Create a link to the file
      downloadLink.href = window.URL.createObjectURL(csvFile);

      // Hide download link
      downloadLink.style.display = "none";

      // Add the link to DOM
      document.body.appendChild(downloadLink);

      // Click download link
      downloadLink.click();
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@700&display=swap");

strong {
  font-family: "Montserrat", sans-serif;
  font-size: 14px;
}

.font,
label {
  font-family: "Poppins", sans-serif;
  color: black;
  font-size: 14px;
}

.pointer {
  cursor: pointer;
}

.borde {
  border: 2px solid rgb(232, 140, 250);
  padding: 20px;
  border-radius: 8px;
}

/* Table */
table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 1px solid rgb(233, 49, 233);
  /* font-family: 'Montserrat', sans-serif; */
  
}

thead th:nth-child(1) {
  width: 30%;
}

tr {
  margin-top: 0px;
  border: 1px solid rgb(112, 169, 255);
  padding: 128px !important;
}

th {
  text-align: left;
  color: #4679bd;
}

th,
td {
  padding: 10px;
  font-family: 'Poppins', sans-serif;
}
</style>