<template>
  <div class="text-center">
    <v-menu offset-y top nudge-top="6" :close-on-content-click="false">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="accent" dark v-bind="attrs" v-on="on">
          <v-icon left>
            {{ $globals.icons.fileImage }}
          </v-icon>
          {{ $t("general.image") }}
        </v-btn>
      </template>
      <v-card width="400">
        <v-card-title class="headline flex mb-0">
          <div>
            {{ $t("recipe.recipe-image") }}
          </div>
          <TheUploadBtn
            class="ml-auto"
            url="none"
            file-name="image"
            :text-btn="false"
            @uploaded="uploadImage"
            :post="false"
          />
        </v-card-title>
        <v-card-text class="mt-n5">
          <div>
            <v-text-field :label="$t('general.url')" class="pt-5" clearable v-model="url" :messages="getMessages()">
              <template v-slot:append-outer>
                <v-btn class="ml-2" color="primary" @click="getImageFromURL" :loading="loading" :disabled="!slug">
                  {{ $t("general.get") }}
                </v-btn>
              </template>
            </v-text-field>
          </div>
        </v-card-text>
      </v-card>
    </v-menu>
  </div>
</template>

<script>
const REFRESH_EVENT = "refresh";
const UPLOAD_EVENT = "upload";
import TheUploadBtn from "@/components/UI/Buttons/TheUploadBtn";
import { api } from "@/api";
export default {
  components: {
    TheUploadBtn,
  },
  props: {
    slug: String,
  },
  data: () => ({
    url: "",
    loading: false,
  }),
  methods: {
    uploadImage(fileObject) {
      this.$emit(UPLOAD_EVENT, fileObject);
    },
    async getImageFromURL() {
      this.loading = true;
      if (await api.recipes.updateImagebyURL(this.slug, this.url)) {
        this.$emit(REFRESH_EVENT);
      }
      this.loading = false;
    },
    getMessages() {
      return this.slug ? [""] : [this.$i18n.t("recipe.save-recipe-before-use")];
    },
  },
};
</script>

<style lang="scss" scoped></style>
