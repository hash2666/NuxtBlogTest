<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-center">
        <logo />
        <vuetify-logo />
      </div>
    </v-flex>

    <v-card v-for="work in works" :key="work.sys.id">
      <v-card class="d-flex flex-row mb-3" />
      <v-img
        height="250px"
        width="500px"
        :style="
          'background-image: url(' +
            work.fields.image.fields.file.url +
            '); background-size: cover;'
        "
      ></v-img>

      <v-card-title>
        <h4>{{ work.fields.title }}</h4>
      </v-card-title>

      <v-card-subtitle>
        <h4>{{ work.fields.subtitle }}</h4>
      </v-card-subtitle>

      <v-card-actions>
        <v-btn text>Share</v-btn>

        <v-btn color="purple" text>
          Explore
        </v-btn>

        <v-spacer></v-spacer>

        <v-btn icon @click="show = !show">
          <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>

          <v-card-text v-for="tag in work.fields.tag" :key="tag.sys.id">
            {{ tag.fields.name }}
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </v-layout>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";
import { createClient } from "~/plugins/contentful.js";
const client = createClient();
export default {
  components: {
    Logo,
    VuetifyLogo
  },
  asyncData() {
    return Promise.all([
      client.getEntries({
        content_type: "work", // workタイプの記事データを全取得
        order: "-sys.createdAt" // 作成日時順に並べる
      })
    ])
      .then(([works]) => {
        return {
          works: works.items // 取得したデータを配列worksに入れる
        };
      })
      .catch(console.error);
  }
};
</script>
