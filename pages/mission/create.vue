<template>
  <v-layout>
    <v-flex xs12 sm12 md12>
      <v-container>
        <v-row>
          <v-col>
            <h1>Mission Management Create</h1>
          </v-col>
          <v-spacer></v-spacer>
          <v-col class="text-right">
            <nuxt-link to="/mission" style="text-decoration:none;">
              <v-btn color="warning">ย้อนกลับ</v-btn>
            </nuxt-link>
          </v-col>
        </v-row>
      </v-container>
      <v-card class="pa-10">
        <v-form ref="form">
          <v-select
            item-value="id"
            item-text="artist_name"
            :items="artistLists"
            label="ศิลปิน"
            v-model="artist_id"
            solo
          ></v-select>
          <v-text-field
            label="Solo"
            placeholder="บูส"
            type="number"
            solo
            v-model="boosts"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="บูสที่ต้องใช้"
            type="number"
            solo
            v-model="boosts_need"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="id หมวดหมู่"
            type="number"
            solo
            v-model="category_id"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="mission"
            type="text"
            solo
            v-model="mission"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="mission_desc"
            type="text"
            solo
            v-model="mission_desc"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="mission_end"
            type="text"
            solo
            v-model="mission_end"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="mission_start"
            type="text"
            solo
            v-model="mission_start"
          ></v-text-field>
          <v-text-field
            label="Solo"
            placeholder="ชื่อ"
            type="text"
            solo
            v-model="name"
          ></v-text-field>
          <label>รูปหลัก</label>
          <image-upload @image="MainImageUpload"></image-upload>
          <label>รูป mission</label>
          <image-upload @image="MissionImageUpload"></image-upload>
          <v-btn class="mt-5" @click="addMission()" color="success" block>ยืนยัน</v-btn>
        </v-form>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from "axios";
export default {
  mounted() {
    this.getArtistList();
  },
  data() {
    return {
      artist_id: "",
      boosts: "",
      boosts_need: "",
      enabled: true,
      category_id: "",
      image_url: "",
      mission: "",
      mission_desc: "",
      mission_image_url: "",
      mission_start: "",
      mission_end: "",
      name: "",
      artistLists: []
    };
  },
  methods: {
    MainImageUpload(image) {
      this.image_url = image;
    },
    MissionImageUpload(image) {
      this.mission_image_url = image;
    },
    getArtistList() {
      axios
        .post(
          "https://us-central1-star-booster-ais-new-bis.cloudfunctions.net/get_artist",
          "",
          {
            headers: {
              "Content-type": "application/json",
              Authorization: "Basic YWlzc3RhcmJvb3N0ZXI6Ym9vc3RlcmFpc0AyMDE5"
            }
          }
        )
        .then(response => {
          if (response.data.code == 0) {
            this.artistLists = response.data.data;
          }
        });
    },
    addMission() {
      axios
        .post(
          "https://us-central1-star-booster-ais-new-bis.cloudfunctions.net/add_mission",
          {
            artist_id: this.artist_id,
            boosts: this.boosts,
            boosts_need: this.boosts_need,
            enabled: this.enabled,
            category_id: this.category_id,
            image_url: this.image_url,
            mission: this.mission,
            mission_desc: this.mission_desc,
            mission_image_url: this.mission_image_url,
            mission_start: this.mission_start,
            name: this.name
          },
          {
            headers: {
              "Content-type": "application/json",
              Authorization: "Basic YWlzc3RhcmJvb3N0ZXI6Ym9vc3RlcmFpc0AyMDE5"
            }
          }
        )
        .then(response => {
          console.log(response.data.code);
          if (response.data.code == 0) {
            this.$router.push("/mission");
          }
        });
    }
  }
};
</script>
