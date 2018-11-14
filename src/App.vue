<template>
  <div id="app">
    <h1 style="text-align:center;">V-EDIT-DIALOG</h1>
    <v-app id="inspire">
      <div> 
        <v-container>
          <v-layout
            row
            wrap
            justify-center>
            <v-flex xs2>
              <v-switch
                v-model="sima_dark"
                label="Sötét téma(táblán) - Aktiválja a sötét témát." />
            </v-flex>
            <v-flex xs2>
              <v-switch
                v-model="dark_edit"
                label="Sötét téma(dialóguson) - Aktiválja a sötét témát." />
            </v-flex>
            <v-flex xs2>
              <v-switch
                v-model="persistent"
                label="Persistent - Nem lehet megszakítani az ablakból való kikattintással." />
            </v-flex>
            <v-flex xs2>
              <v-switch
                v-model="large"
                label="Large  - Aktiválja a Save és Cancel gombokat." />
            </v-flex>
            <v-flex xs2>
              <v-switch
                v-model="savetext"
                label="Save-text  - Alternativ szöveget lehet belállítani(csak a Large aktiválásával működik)." />
            </v-flex>
          </v-layout>
        </v-container>

        <v-container>
          <v-data-table
            :headers="headers"
            :items="desserts"
            :dark="sima_dark">
            <template 
              slot="items" 
              slot-scope="props">
              <td>
                <v-edit-dialog
                  :return-value.sync="props.item.name"
                  :dark="dark_edit"
                  :large="large"
                  :persistent="persistent"
                  :save-text="savetext"
                  @save="save"
                  @cancel="cancel"
                  @open="open"
                  @close="close"> {{ props.item.name }}
                  <v-text-field
                    slot="input"
                    v-model="props.item.name"
                    :rules="[max25chars]"
                    label="Edit"
                    single-line
                    counter />
                </v-edit-dialog>
              </td>
              <td class="text-xs-right">{{ props.item.mezo_1 }}</td>
              <td class="text-xs-right">{{ props.item.mezo_2 }}</td>
              <td class="text-xs-right">{{ props.item.mezo_3 }}</td>
              <td class="text-xs-right">{{ props.item.mezo_4 }}</td>
            </template>
          </v-data-table>
      
          <v-snackbar 
            v-model="snack" 
            :timeout="3000" 
            :color="snackColor">
            {{ snackText }}
            <v-btn 
              flat 
              @click="snack = false">Bezárás</v-btn>
          </v-snackbar>
        </v-container>
      </div>
    </v-app>
    <h3 style="text-align: right">Készítette: Németh Ernő Nándor </h3>
    
  </div>
</template>
<script>
export default {
  data() {
    return {
      snack: false,
      snackColor: "",
      snackText: "",
      max25chars: v => v.length <= 25 || "Túl sok karakter! Maximum 25!",
      pagination: {},
      headers: [
        {
          text: "Nevek",
          align: "left",
          sortable: false,
          value: "name"
        },
        { text: "1. mező", value: "mezo_1" },
        { text: "2. mező", value: "mezo_2" },
        { text: "3. mező", value: "mezo_3" },
        { text: "4. mező ", value: "mezo_4" }
      ],
      desserts: [
        {
          value: false,
          name: "Teszt Elek",
          mezo_1: 159,
          mezo_2: 6.0,
          mezo_3: 24,
          mezo_4: 4.0
        },
        {
          value: false,
          name: "Gipsz Jakab",
          mezo_1: 237,
          mezo_2: 9.0,
          mezo_3: 37,
          mezo_4: 4.3
        },
        {
          value: false,
          name: "Jakab József",
          mezo_1: 262,
          mezo_2: 16.0,
          mezo_3: 23,
          mezo_4: 6.0
        },
        {
          value: false,
          name: "Példa Áron",
          mezo_1: 305,
          mezo_2: 3.7,
          mezo_3: 67,
          mezo_4: 4.3
        },
        {
          value: false,
          name: "John Wick",
          mezo_1: 356,
          mezo_2: 16.0,
          mezo_3: 49,
          mezo_4: 3.9
        }
      ]
    };
  },
  methods: {
    save() {
      this.snack = true;
      this.snackColor = "success";
      this.snackText = "Mentve";
    },
    cancel() {
      this.snack = true;
      this.snackColor = "error";
      this.snackText = "Megszakítva";
    },
    open() {
      this.snack = true;
      this.snackColor = "info";
      this.snackText = "Megnyitva";
    },
    close() {}
  }
};
</script>
