<template>
  <v-sheet class="mx-auto" width="300">
    <v-form v-model="valid" fast-fail :key="formKey" ref="form" @submit.prevent="saveMusic">
      <v-text-field
        v-model="music.name"
        :rules="triedToSubmit ? stringRules : []"
        label="Nome da música"
      ></v-text-field>

      <v-text-field
        v-model="music.style"
        :rules="triedToSubmit ? stringRules : []"
        label="Estilo musical"
      ></v-text-field>

      <v-select
        v-model="music.score"
        :items="items"
        :rules="triedToSubmit ? itemsRules : []"
        label="Pontuação"
      ></v-select>

      <v-btn class="mt-2" type="submit" block :disabled="!valid">Cadastrar</v-btn>
    </v-form>
  </v-sheet>

  <v-table>
    <thead>
      <tr>
        <th class="text-left">
          Nome da música
        </th>
        <th class="text-left">
          Estilo Musical
        </th>
        <th class="text-left">
          Pontuação
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="item in musics"
        :key="item.name"
      >
        <td>{{ item.name }}</td>
        <td>{{ item.style }}</td>
        <td>{{ item.score }}</td>
      </tr>
    </tbody>
  </v-table>

  <!-- Dialog -->
  <v-dialog max-width="500" v-model="isActive">
      <v-card title="Erro">
        <v-card-text>
        A musica informada já foi cadastrada
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            text="Fechar"
            @click="isActive = false"
          ></v-btn>
        </v-card-actions>
      </v-card>
  </v-dialog>

</template>

<script>
  export default {
    data: () => ({
      triedToSubmit: false,
      musics: [],
      music: {name: '', style: '', score: null},
      isActive: false,
      stringRules: [
        value => {
          if (value) return true
          return 'O campo não pode estar vazio'
        },
      ],
      itemsRules: [
        value => {
          if (value) return true
          return 'O campo não pode estar vazio'
        }
      ],
      items: [
        '1',
        '2',
        '3',
        '4',
        '5'
      ],
    }),
    methods: {
      saveMusic() {
        this.triedToSubmit = true;
        if (this.$refs.form.validate()) {
          if(!this.musics.some(music => music.name == this.music.name)){
            this.musics.push({ ...this.music }); 
            this.resetForm(); 
          } else {
            this.isActive = true;
          }
        }
      },
      resetForm() {
        this.music = { name: '', style: '', score: null };
        this.$refs.form.resetValidation();
        this.triedToSubmit = false;
      },
    }
}
</script>