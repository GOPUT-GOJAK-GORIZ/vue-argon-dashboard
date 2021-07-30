<template>
  <div>
    <base-header
      class="header pb-8 pt-5 pt-lg-7 d-flex align-items-center"
      style="
        min-height: 300px;
        background-size: cover;
        background-position: center top;
      "
    >
      <!-- Mask -->
      <span class="mask bg-gradient-success opacity-8"></span>
      <!-- Header container -->
      <div class="container-fluid d-flex align-items-center">
        <div class="row">
          <div class="col-lg-10 col-md-30">
            <h1 class="display-2 text-white">Antar Barang</h1>
          </div>
        </div>
      </div>
    </base-header>

    <div class="container-fluid mt--7">
      <div class="row">
        <div class="col-xl-8 order-xl-1">
          <card shadow type="secondary">
            <form>
              <h6 class="heading-small text-muted mb-4">Antar Barang</h6>
              <div class="pl-lg-4">
                <div class="row ml-1">
                  <div class="col-lg-6 mb-3">
                    <p>Where are you now?</p>
                    <label for="">Address</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="start_loc.address"
                    />

                    <div
                      class="btn btn-info mt-3 mb-3"
                      @click="searchStartLoc(start_loc.address)"
                    >
                      Set Start Loc
                    </div>

                    <div
                      id="map"
                      class="map-canvas"
                      :data-lat="start_loc.latitude"
                      :data-lng="start_loc.longitude"
                      style="height: 300px"
                    ></div>

                    <br />

                    <label for="">Latitude</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="start_loc.latitude"
                    />
                    <label for="">Longitude</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="start_loc.longitude"
                    />
                  </div>
                  <div class="col-lg-6 mb-3">
                    <p>Where do you want to send the package?</p>
                    <label for="">Address</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="end_loc.address"
                    />

                    <div
                      class="btn btn-info mt-3 mb-3"
                      @click="searchEndLoc(end_loc.address)"
                    >
                      Set End Loc
                    </div>

                    <div
                      id="map2"
                      class="map-canvas"
                      :data-lat="end_loc.latitude"
                      :data-lng="end_loc.longitude"
                      style="height: 300px"
                    ></div>
                    <br />

                    <label for="">Latitude</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="end_loc.latitude"
                    />
                    <label for="">Longitude</label>
                    <input
                      type="text"
                      class="form-control mb-1"
                      v-model="end_loc.longitude"
                    />
                  </div>
                </div>
                <br />
                <hr class="my-4" />
                <h6 class="heading-small text-muted mb-4">Recepient Details</h6>
                <div class="pl-lg-4">
                  <div class="row">
                    <div class="col-lg-6 mb-3">
                      <label for="verify" class="col-sm-50">
                        Recepient's Name
                      </label>

                      <input
                        type="text"
                        class="form-control"
                        placeholder="Recepient's name"
                        v-model="recipient_detail.name"
                      />
                    </div>
                    <div class="col-lg-6 mb-3">
                      <label for="verify" class="col-sm-50">
                        Recepient's Phone Number
                      </label>
                      <input
                        type="text"
                        class="form-control"
                        placeholder="08XXXXXXXXX"
                        v-model="recipient_detail.phone_number"
                      />
                    </div>
                  </div>
                </div>
                <br />
                <hr class="my-4" />
                <h6 class="heading-small text-muted mb-4">Item Details</h6>
                <div class="pl-lg-4">
                  <div class="row">
                    <div class="col-lg-6 mb-3">
                      <label for="verify" class="col-sm-50"> Weight </label>

                      <input
                        type="text"
                        class="form-control"
                        placeholder="xx gram"
                        v-model="item_detail.weight"
                      />
                    </div>
                    <div class="col-lg-6 mb-3">
                      <label for="verify" class="col-sm-50"> Type </label>
                      <input
                        type="text"
                        class="form-control"
                        placeholder="food,stuff,etc"
                        v-model="item_detail.type"
                      />
                    </div>
                    <div class="col-lg-6 mb-3">
                      <label for="verify" class="col-sm-50">
                        Delivery Instruction
                      </label>
                      <input
                        type="text"
                        class="form-control"
                        placeholder="be careful with package"
                        v-model="item_detail.delivery_instruction"
                      />
                    </div>
                  </div>
                </div>
              </div>
              <div class="btn btn-info mt-3" @click="createOrder">
                Order Now!
              </div>
            </form>
          </card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import http from "../../http.js";
import axios from "axios";
import { Loader } from "google-maps";
const loader = new Loader("AIzaSyAvNJNQgq8y58I7Uag7pVQr0W6moI3LtQI");

export default {
  name: "user-order",
  data() {
    return {
      model: {},
      start_loc: {
        address: "",
        latitude: "",
        longtitude: "",
      },
      end_loc: {
        address: "",
        latitude: "",
        longtitude: "",
      },
      item_detail: {
        weight: 0,
        type: "",
        delivery_instruction: "",
      },
      recipient_detail: {
        recipient_name: "",
        recipient_phone_number: "",
      },
    };
  },
  mounted() {
    this.loadMaps();
  },
  methods: {
    loadMaps() {
      loader.load().then(function (google) {
        // Regular Map
        const myLatlng = new google.maps.LatLng(40.748817, -73.985428);
        const mapOptions = {
          zoom: 13,
          center: myLatlng,
          scrollwheel: false, // we disable de scroll over the map, it is a really annoing when you scroll through page
          disableDefaultUI: true, // a way to quickly hide all controls
          zoomControl: true,
          styles: [
            {
              featureType: "administrative",
              elementType: "labels.text.fill",
              stylers: [{ color: "#444444" }],
            },
            {
              featureType: "landscape",
              elementType: "all",
              stylers: [{ color: "#f2f2f2" }],
            },
            {
              featureType: "poi",
              elementType: "all",
              stylers: [{ visibility: "off" }],
            },
            {
              featureType: "road",
              elementType: "all",
              stylers: [{ saturation: -100 }, { lightness: 45 }],
            },
            {
              featureType: "road.highway",
              elementType: "all",
              stylers: [{ visibility: "simplified" }],
            },
            {
              featureType: "road.arterial",
              elementType: "labels.icon",
              stylers: [{ visibility: "off" }],
            },
            {
              featureType: "transit",
              elementType: "all",
              stylers: [{ visibility: "off" }],
            },
            {
              featureType: "water",
              elementType: "all",
              stylers: [{ color: "#5e72e4" }, { visibility: "on" }],
            },
          ],
        };
        const map = new google.maps.Map(
          document.getElementById("map"),
          mapOptions
        );

        const map2 = new google.maps.Map(
          document.getElementById("map2"),
          mapOptions
        );
        const marker = new google.maps.Marker({
          position: myLatlng,
          title: "Regular Map!",
        });
        marker.setMap(map);
        marker.setMap(map2);
      });
    },

    searchStartLoc(address) {
      const url =
        "https://api.opencagedata.com/geocode/v1/json?key=3ff7bb143a9d46b99978fd40bad99cef&q=" +
        address;
      axios
        .get(url, {
          headers: {
            "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
          },
        })
        .then((response) => {
          console.log(response);
          this.start_loc.latitude = response.data.results[0].geometry.lat;
          this.start_loc.longitude = response.data.results[0].geometry.lng;
        });
    },
    searchEndLoc(address) {
      const url =
        "https://api.opencagedata.com/geocode/v1/json?key=3ff7bb143a9d46b99978fd40bad99cef&q=" +
        address;
      axios
        .get(url, {
          headers: {
            "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
          },
        })
        .then((response) => {
          console.log(response);
          this.end_loc.latitude = response.data.results[0].geometry.lat;
          this.end_loc.longitude = response.data.results[0].geometry.lng;
        });

      const distance = this.getDistance(this.start_loc, this.end_loc);
      // alert(distance);
      this.getPrice(distance);
    },

    getPrice(dist) {
      // alert(dist);
      this.price = Math.round((dist * 2500) / 1000);
    },

    rad(x) {
      return (x * Math.PI) / 180;
    },

    getDistance(p1, p2) {
      p1 = this.start_loc;
      p2 = this.end_loc;
      var R = 6378137; // Earth’s mean radius in meter
      var dLat = this.rad(p2.latitude - p1.latitude);
      var dLong = this.rad(p2.longitude - p1.longitude);
      var a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(this.rad(p1.latitude)) *
          Math.cos(this.rad(p2.latitude)) *
          Math.sin(dLong / 2) *
          Math.sin(dLong / 2);
      var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
      var d = R * c;
      return d / 1000; // returns the distance in kilo meter
    },
    createOrder() {
      let formData = {
        id_customer: localStorage.getItem("customer_id"),
        type_of_service: "Transportasi Motor",
        start_loc: {
          longtitude: this.start_loc.longitude,
          latitude: this.start_loc.latitude,
        },
        end_loc: {
          longtitude: this.end_loc.longitude,
          latitude: this.end_loc.latitude,
        },
        item_detail: {
          weight: this.item_detail.weight,
          type: this.item_detail.type,
          delivery_instruction: this.item_detail.delivery_instruction,
      },
      recipient_detail: {
          recipient_name: this.recipient_detail.recipient_name,
          recipient_phone_number: this.recipient_detail.recipient_phone_number,
      },

        price: this.price,
      };

      const jsonData = JSON.stringify(formData);

      const url = "/cust/create/order/antarbarang";

      http
        .post(url, jsonData)
        .then((response) => {
          if (response.status == 201) {
            alert("Your order successfully created!");
          }

          this.$router.push({
            name: "Activity History",
          });
        })
        .catch((error) => {
          alert("Failed to create order \n" + error);
        });
    },
  },
};
</script>
<style></style>
