<template>
  <v-app>
    <v-app-bar
      dense
      app
      color="primary"
      dark
    >
      <v-toolbar-title>
        GET MY GRAVATAR
      </v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container class="fill-height" fluid>
        <v-row justify="center" align="center">
          <v-col cols="12" sm="6" md="4">
            <v-card elevation="0">
              <template v-if="url">
                <v-card-text class="d-flex justify-center">
                  <v-img :src="url" lazy-src="https://www.gravatar.com/avatar/a.jpg?d=mp&s=480" width="100%">
                    <template v-slot:placeholder>
                      <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                      >
                        <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                      </v-row>
                    </template>
                  </v-img>
                </v-card-text>
              </template>
              <v-form ref="form" @submit.prevent="createHash" :disabled="loading">
                <v-text-field solo dense v-model="hash" placeholder="이름을 입력하세요." :rules="[v => !!v || '꼭 입력해주셔야합니다.']"></v-text-field>
                <v-btn color="success" block type="submit">만들기</v-btn>
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-footer app color="primary" dark class="d-flex justify-center">
      <v-btn text href="https://ko.gravatar.com/">
        https://ko.gravatar.com/
        <v-icon right>mdi-open-in-new</v-icon>
      </v-btn>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Ref } from 'vue-property-decorator'
import CryptoJS from 'crypto-js'

@Component({
  name: 'App'
})
export default class App extends Vue {
  url = 'https://www.gravatar.com/avatar/a.jpg?d=mp&s=480'
  hash = ''
  loading = false

  @Ref('form') form!: { validate: () => boolean }

  createHash () {
    if (!this.form.validate()) return
    const hash = CryptoJS.MD5(this.hash).toString()
    this.url = `https://www.gravatar.com/avatar/${hash}.jpg?d=robohash&s=480`
  }
}
</script>
