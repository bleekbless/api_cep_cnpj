<template>
  <v-app light>
    <v-toolbar fixed>
      <v-toolbar-title v-text="title"></v-toolbar-title>
    </v-toolbar>
    <main>
      <v-container fluid>
        <v-slide-y-transition mode="out-in">
          <v-layout row align-center>
            <v-flex xs4 sm4 offset-sm2 class="mr-3">
              <v-card>
                <v-card-title secondary-title>
                  Pesquise seu CEP
                </v-card-title>
                <v-card-text>
                  <v-form v-model="validCEP" @submit.prevent="buscarCEP()" ref="form">
                    <v-text-field class="cep-element" label="Digite aqui seu CEP" v-model="cep" :rules="CEPRules" :counter="8"></v-text-field>

                    <v-btn @click="buscarCEP()" class="white--text" :class="{ green: validCEP, allow: validCEP, red: !validCEP, notAllow: !validCEP  }">Pesquisar</v-btn>
                  </v-form>
                </v-card-text>
                <v-card-text v-if="resultCEP">
                  <div>
                    <h3 class="headline mb-0">Logradouro: {{ responseCEP.logradouro }}</h3>
                    <div>
                      <b>Complemento: </b> {{ responseCEP.complemento || 'Sem complemento'}}
                    </div>
                    <div>
                      <b>Bairro: </b> {{ responseCEP.bairro || 'Sem bairro' }}
                    </div>
                    <div>
                      <b>Cidade: </b> {{ responseCEP.localidade || 'Sem Cidade' }}
                    </div>
                    <div>
                      <b>UF: </b> {{ responseCEP.uf || 'Sem UF' }}
                    </div>

                  </div>
                </v-card-text>
              </v-card>

            </v-flex>
            <v-flex xs4 sm4>
              <v-card>
                <v-card-title secondary-title>
                  Pesquise seu CNPJ
                </v-card-title>
                <v-card-text>
                  <v-form v-model="validCNPJ" @submit.prevent="buscarCNPJ()" ref="form">
                    <v-text-field label="Digite aqui o CNPJ" v-model="cnpj" :rules="CNPJRules" :counter="10" required></v-text-field>

                    <v-btn @click="buscarCNPJ()" class="white--text" :class="{ green: validCNPJ, allow: validCNPJ, red: !validCNPJ, notAllow: !validCNPJ }">Pesquisar</v-btn>
                  </v-form>
                </v-card-text>
                <v-card-text v-if="resultCNPJ">
                  <div>
                    <h3 class="headline mb-0">Nome: {{ responseCNPJ.nome || 'Sem nome' }}</h3>
                    <div>
                      <b>Atividade principal</b> {{ responseCNPJ.atividade_principal.text || 'Sem atividade principal' }}
                      </div>
                    <div>
                      <b>Situação</b> {{ responseCNPJ.situacao || 'Sem situação' }}
                      </div>
                    <div>
                      <b>Data de Abertura</b> {{ responseCNPJ.abertura || 'Sem data de abertura' }}
                      </div>
                    <div>
                      <b>Nome fantasia</b> {{ responseCNPJ.fantasia || 'Sem nome fantasia' }}
                      </div>
                    <div>
                      <b>Tipo</b> {{ responseCNPJ.tipo || 'Sem tipo' }}
                      </div>
                  </div>
                   <div>
                     <div>
                      <b>CEP: </b> {{ responseCNPJ.cep || 'Sem CEP'}}
                    </div>
                    <div>
                      <b>Rua: </b> {{ responseCNPJ.logradouro || 'Sem Rua'}}
                    </div>
                    <div>
                      <b>Número: </b> {{ responseCNPJ.numero || 'Sem Número'}}
                    </div>
                    <div>
                      <b>Complemento: </b> {{ responseCNPJ.complemento || 'Sem complemento'}}
                    </div>
                    <div>
                      <b>Bairro: </b> {{ responseCNPJ.bairro || 'Sem Bairro' }}
                    </div>
                    <div>
                      <b>Cidade: </b> {{ responseCNPJ.municipio || 'Sem Cidade' }}
                    </div>
                    <div>
                      <b>UF: </b> {{ responseCNPJ.uf || 'Sem UF' }}
                    </div>

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
import axios from 'axios'
import _ from 'lodash'
import Cleave from 'cleave.js'

export default {
  created() {
    
    let cleave = new Cleave('.cep-element', {
      delimiters: ['.', '.', '-'],
      blocks: [3, 3, 3, 2],
      uppercase: true
    });
  },
  data() {
    return {
      items: [
        { icon: 'bubble_chart', title: 'Inspire' }
      ],
      validCEP: false,
      validCNPJ: false,
      CEPRules: [
        (v) => v && v.length === 8 || 'CEP deve ter exatamente 8 caracteres'
      ],
      CNPJRules: [
        (v) => v && v.length == 14 || 'CEP deve ter exatamente 14 caracteres'
      ],
      cep: '',
      cnpj: '',
      title: 'Pesquise seu CEP e seu CNPJ',
      resultCEP: false,
      responseCEP: {},
      resultCNPJ: false,
      responseCNPJ: {}
    }
  },
  methods: {
    buscarCEP() {
      axios.get(`https://viacep.com.br/ws/${this.cep}/json/`).then((response) => {
        this.responseCEP = response.data
        this.resultCEP = true
      }).catch((error) => {
        console.log(error)
      })
    },
    buscarCNPJ() {
      // let regex = /\b(\.|\/|\-)\b/
     // this.cnpj = _.replace(this.cnpj, regex, '')
      axios.get(`https://www.receitaws.com.br/v1/cnpj/${this.cnpj}`).then((response) => {
        this.responseCNPJ = response.data
        this.resultCNPJ = true
      }).catch((error) => {
        console.log(error)
      })
    }
  }
}
</script>
<style lang="css" scoped>
.notAllow {
  cursor: not-allowed;
}
.allow {
  cursor: pointer;
}
</style>
