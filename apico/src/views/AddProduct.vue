<template>
  <div class="content">
    <div class="form big-form">
      <div class="formContent">
        <h3>Add product</h3>
        <div>
          <label for="title" class="important">Title</label>
          <input
            v-model="title"
            id="title"
            type="text"
            @change="userError = null"
            placeholder="For example: Iron man suit"
          />

          <label for="location" class="important">Location</label>
          <input
            v-model="location"
            id="location"
            type="text"
            @change="userError = null"
            placeholder="For example: Los Angeles, CA"
          />

          <label for="description">Description</label>
          <input
            v-model="description"
            id="description"
            type="text"
            placeholder="description"
          />

          <div>
            <label for="photo">Photo</label>
            <input
              v-show="false"
              id="photo"
              type="file"
              ref="inputFile"
              class="inputFile"
              @change="previewImage"
              accept="image/"
            />
            <div class="addPicture">
              <img v-if="previewUrl" class="preview" :src="previewUrl" />
              <div class="iconBox" @click="OpenFileSelect()">
                <img src="../assets/plus.svg" />
              </div>
            </div>
          </div>

          <label for="price">Price</label>
          <input
            v-model.number="price"
            id="price"
            type="text"
            placeholder="Price"
          />
          <p class="error">{{ error }}</p>
          <p v-if="userError" class="error">{{ userError }}</p>
          <div class="btn submit" v-if="!loading" @click="submit()">SUBMIT</div>
          <div class="btn submit disabled" v-else>Loading...</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import firestore from "firebase/app";
export default {
  name: "Home",
  data() {
    return {
      title: "",
      location: "",
      description: "",
      price: 0,
      inputFile: this.$refs.inputFile,
      imageData: "",
      previewUrl: null,
      userError: null,
    };
  },
  methods: {
    submit() {
      if (this.title.length < 1 && this.location.length < 1) {
        this.userError = "please fill out all required fields";
        return;
      }
      const productObj = {
        title: this.title,
        description: this.description,
        imageData: this.imageData,
        price: this.price,
        location: this.location,
      };
      this.$store.dispatch("saveProduct", productObj);
    },
    OpenFileSelect() {
      this.inputFile.click();
    },
    previewImage(event) {
      this.imageData = event.target.files[0];
      this.previewUrl = URL.createObjectURL(this.imageData);
    },
  },
  mounted() {
    this.inputFile = this.$refs.inputFile;
  },
  computed: {
    loading() {
      return this.$store.getters.loading;
    },
    error() {
      return this.$store.getters.error;
    },
  },
  created() {
    this.$store.commit("avatar", true);
  },
  beforeDestroy() {
    this.$store.commit("avatar", false);
  },
};
</script>
<style lang="scss">
@import "./../scss/_addProduct.scss";
</style>;

