<template>
    <v-container fluid style="padding: 0px">
        <v-main class="main-app">
            <v-data-table
            v-model="selected"
            :headers="headers"
            :items="desserts"
            :search="search"
            item-key="user"
            show-select
            hide-default-footer
            class="table elevation-1"
            > 
            <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-dialog
            v-model="dialog"
            max-width="500px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
              >
                NOVO USUÁRIO
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>
  
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.user"
                        label="USUÁRIO"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.email"
                        label="EMAIL"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.inc_date"
                        label="DATA DE INCLUSÃO"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.alt_date"
                        label="DATA DE ALTERAÇÃO"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.rules"
                        label="REGRAS"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.status"
                        label="STATUS"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
  
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="close()"
                >
                  Cancelar
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="save()"
                >
                  Salvar
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete()">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm()">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>


                  <template v-slot:[`item.action`]="{ item }">
                        <span v-if="seen">
                        <v-icon
                        small
                        class="mr-2"
                        @click="editItem(item)"
                        >
                        mdi-pencil
                        </v-icon>
                        <v-icon
                        class="mr-2"
                        small
                        @click="deleteItem(item)"
                        >
                        mdi-delete
                        </v-icon>
                        </span>
                        <v-icon
                        small
                        @click="showOption()"
                        >
                        mdi-dots-horizontal
                        </v-icon>
                    </template>
                
            </v-data-table>


        <v-pagination
            v-model="page"
            :length="pageCount"
            first-icon="Primeiro"
            :footer-props= "{showFirstLastPage: true}"
            color="#D83367"
        ></v-pagination>
        </v-main>
<!--            Table -->
    </v-container>
</template>

<script>
import Data from '../services/data'

export default {
    data:()=>{
        return{
            dialog: false,
            dialogDelete: false,
            page: 1,
            pageCount: 0,
            itemsPerPage: 10,
            seen: false,
            selected: [],
            headers: [
                {
                text: 'USUÁRIO',
                align: 'center',
                value: 'user',
                class: 'custom-header'
                },
                { text: 'EMAIL', value: 'email' },
                { text: 'DATA DE INCLUSÃO', align: 'center', value: 'inc_date' },
                { text: 'DATA DE ALTERAÇÃO', align: 'center', value: 'alt_date' },
                { text: 'REGRAS', align: 'center', value: 'rules' },
                { text: 'STATUS', align: 'center', color:'#31BA1F', value: 'status' },
                { text: 'AÇÕES', align: 'right', value: 'action', sortable: false}
            ],
            desserts: [],
            editedIndex: -1,
            editedItem: {
            user: '',
            email: '',
            inc_date: '',
            alt_date: '',
            rules: '',
            status: '',
            },
            defaultItem: {
            user: '',
            email: '',
            inc_date: '',
            alt_date: '',
            rules: '',
            status: '',
            },
        }
    },
                methods:{
                showOption(item){
                    console.log(item);
                    console.log(this.desserts.indexOf(item))
                    console.log(this.editedItem = Object.assign({}, item));
                    this.seen = !this.seen;
                },
                editItem (item) {
                    this.editedIndex = this.desserts.indexOf(item)
                    this.editedItem = Object.assign({}, item)
                    this.dialog = true
                },
                deleteItem (item) {
                    this.editedIndex = this.desserts.indexOf(item)
                    this.editedItem = Object.assign({}, item)
                    this.dialogDelete = true
                },
                deleteItemConfirm () {
                    this.desserts.splice(this.editedIndex, 1)
                    this.closeDelete()
                },
                close () {
                    this.dialog = false
                    this.$nextTick(() => {
                        this.editedItem = Object.assign({}, this.defaultItem)
                        this.editedIndex = -1
                        })
                },
                closeDelete () {
                    this.dialogDelete = false
                    this.$nextTick(() => {
                        this.editedItem = Object.assign({}, this.defaultItem)
                        this.editedIndex = -1
                    })
                },
                save () {
                    if (this.editedIndex > -1) {
                        Object.assign(this.desserts[this.editedIndex], this.editedItem)
                    } else {
                        this.desserts.push(this.editedItem)
                    }
                    this.close()
                    },
            },
    props:{
        search: String,
        dialogBox: Boolean,
    },
    mounted(){
        Data.list().then(response => {
        console.log(response)
        this.desserts = response.data
        });
    },
    watch: {
        dialog (val) {
        val || this.close()
        },
        dialogDelete (val) {
        val || this.closeDelete()
        },
    },
    computed: {
        formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
        },
    },
}
</script>

<style>
.conteiner{
    padding: 0px;
}
.main-app{
  background-color: #F5F5F5;
}
.table{
  margin-top: 20px;
  margin-left: 40px;
  margin-right: 40px;
  font-weight: 500;
}
.head{
  font-weight: 500;
}

tr:nth-child(even) {
    background-color: #E9E9E9 !important;
}
tr:nth-child(odd) {
    background-color: #f5f5f5 !important;
}
.custom-header{
    background-color: white !important;
}


</style>