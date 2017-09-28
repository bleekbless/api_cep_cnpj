<template>
  <v-app light>
    <v-toolbar fixed>
      <v-toolbar-title v-text="title"></v-toolbar-title>
    </v-toolbar>
    <main>
      <v-container fluid>
        <v-slide-y-transition mode="out-in">
          <v-layout column align-center>
            <v-flex xs12 sm6 offset-sm3>
              <v-card>
                <v-card-title secondary-title>
                  Pesquise seu CEP
                </v-card-title>
                <v-card-text>
                  <v-form v-model="valid" ref="form">
                    <v-text-field label="Name" v-model="name" :rules="nameRules" :counter="10" required></v-text-field>

                    <v-btn @click="submit" :class="{ green: valid, red: !valid }">Pesquisar</v-btn>
                  </v-form>
                </v-card-text>
                <v-card-text v-if="result">
                  <div>
                    <h3 class="headline mb-0">Logradouro: {{ 'Rua' }}</h3>
                    <div><b>Located</b> two hours south of Sydney in the Southern Highlands of New South Wales, ...</div>
                    <div><b>Located</b> two hours south of Sydney in the Southern Highlands of New South Wales, ...</div>
                    <div><b>Located</b> two hours south of Sydney in the Southern Highlands of New South Wales, ...</div>
                    <div><b>Located</b> two hours south of Sydney in the Southern Highlands of New South Wales, ...</div>
                    <div><b>Located</b> two hours south of Sydney in the Southern Highlands of New South Wales, ...</div>
                  </div>
                </v-card-text>
              </v-card>

            </v-flex>

          </v-layout>
        </v-slide-y-transition>
      </v-container>
    </main>
    <v-footer :fixed="fixed">
      <span>&copy; 2017</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { icon: 'bubble_chart', title: 'Inspire' }
      ],
      title: 'Pesquise seu CEP e seu CNPJ'
    }
  }
}
</script>
