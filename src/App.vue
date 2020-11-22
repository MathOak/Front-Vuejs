<template>
    <v-app>
      
        <v-navigation-drawer
        v-model="drawer"
        app
        right
        width="480px"
        >
        <v-list-item>
            <v-list-item-content>
            <v-list-item-title class="title"><v-icon>mdi-tune</v-icon>
                FILTROS
            </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
            <v-btn color="#FFFFFF" @click="drawer = !drawer"><v-icon>mdi-close</v-icon></v-btn>
            </v-list-item-action>
        </v-list-item>

        <v-divider></v-divider>

        <v-list-item>
        <v-list-item-content>
            <v-list-item-subtitle class="subtitle">
            Utilize os filtros abaixo para refinar os resultados da tabela, clique no botão APLICAR para salvar as alterações.
            </v-list-item-subtitle>
        </v-list-item-content>
        </v-list-item>

        <v-list
        dense
        nav
        v-for="item in side_itens"
        :key="item.title"
        >
        <v-list-item
        link
        >
            <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
            <v-icon>mdi-menu-down</v-icon>
            </v-list-item-action>

        </v-list-item>

        <v-divider></v-divider>
        </v-list>
        <v-list-item>
            <v-list-item-content>
            <v-btn block height="67" outlined color="#D83367">APLICAR</v-btn>
            </v-list-item-content>
        </v-list-item>
      <!--  -->
        </v-navigation-drawer>
    <!--  Side menu        -->
    <v-app-bar
      color="#F5F5F5"
      app
    >
      <div class="d-flex align-center">
        
        <v-img
          alt="Logo"
          contain
          src=".\assets\symbol-1.svg"
        />
      </div>
      <v-divider inset vertical class="divider"
      ></v-divider>

      <div class="icons d-flex align-center">
        <v-btn color="#FFFFFF" class="button"><v-icon>mdi-security</v-icon></v-btn>
        <v-btn class="button psn" dark><v-icon>mdi-account</v-icon></v-btn>
      </div>
      
      <div class="d-flex align-center">
        <v-text-field
          v-model= "search"
          append-icon="mdi-magnify"
          label="Pesquisar..."
          single-line
          hide-details
          class="search-bar"
        ></v-text-field>

      </div>
      <v-spacer></v-spacer>
      <div class="d-flex align-center">
        <v-btn class="icon-filter" color="#FFFFFF" @click="drawer = !drawer"><v-icon>mdi-tune</v-icon></v-btn>
        <v-btn color="#D83367" dark @click="dialogButton()"><v-icon>mdi-account</v-icon>Incluir Usuário</v-btn>
      </div>
      <v-divider class="divider-right" inset vertical></v-divider>
      <div class="d-flex align-center">
        <v-icon>mdi-home</v-icon>
        <v-icon class="top-right-icon">mdi-cog</v-icon>
        <v-icon>mdi-power</v-icon>
      </div>
      
    </v-app-bar>
<!--  App Bar        -->

      <Table :search="search" :dialogBox="dialogButton()"/>
    
      <v-footer
        absolute
        class="font-weight-medium"
        color="#333333"
        app
      >
        
        <v-row class="footer-prop"><v-icon small color="#FF8700">mdi-radiobox-marked</v-icon>NO AR</v-row>
        <v-row><v-icon small color="#666666">mdi-television</v-icon>Encontro - 10:00</v-row>
        <v-row><v-icon small color="#666666">mdi-timer-outline</v-icon>Última atualização em 10:28</v-row>
        <v-row>{{createDate()}}</v-row>
        <v-row class="footer-prop">// {{createHours()}}</v-row>
      </v-footer>
  </v-app>
</template>

<script>



import Table from './components/Table'
export default {

  name: 'App',

  components: {
    Table
  },


  data: () => ({
    //
    drawer: false,
    search: '',
    crudbox: false,

    side_itens: [
        { title: 'TODAS AS DATAS DE INCLUSÃO', icon: 'mdi-calendar-range' },
        { title: 'TODAS AS DATAS DE ALTERAÇÃO', icon: 'mdi-calendar-range' },
        { title: 'ATIVOS E INATIVOS', icon: 'mdi-toggle-switch' },
    ],
  
    months: ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"],
    day: ["Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"],
    

  }),
 
    methods: {
      menssageReceive(search){
        this.msg = search;
        console.log(this.msg)
      },
      createDate(){
        let d = new Date();
        let date = `${this.day[d.getDay()]}, ${d.getDate()} de ${this.months[d.getMonth()]} de ${d.getFullYear()}`
        return date
      },
      zero(number) {
        if (number < 10) {
            number = '0' + number;
        } return number;
      },
      createHours(){
        let d = new Date();
        let hours = `${d.getHours()}:${this.zero(d.getMinutes())}:${this.zero(d.getSeconds())}`
        return hours
      },

      tableCommand (){
        console.log("Click Funciona");
      },
      dialogButton(){
        return true;
      },
  },
};
</script>

<style>
.divider{
    margin-right: 20px;
}
.divider-right{
  margin: 20px;
}
.button{
  max-height: 48px;
  max-width: 52px;
  background-color: #FFFFFF;
}
.psn{
  background-image: linear-gradient(to top right, #D83367, #FF8700 );
}
.search-bar{
  font-style: italic;
}
.icons{
  margin-right: 30px;
}
.icon-filter{
  margin: 8px;
}
.top-right-icon{
  margin: 20px;
}


.footer-prop{
  font-family: "Audiowide";
  color: #FF8700;
}
.footer-grad{
  background-image: linear-gradient(to top left, #222222, #333333 );
  
}
</style>