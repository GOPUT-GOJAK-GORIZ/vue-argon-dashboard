<template>
  <div>
    <base-header
      class="header pb-8 pt-5 pt-lg-8 d-flex align-items-center"
      style="
        min-height: 200px;
        background-size: cover;
        background-position: center top;
      "
    >
      <!-- Mask -->
      <span class="mask bg-gradient-success opacity-8"></span>
    </base-header>

    <div class="container-fluid mt--7">
      <div class="row">
        <div class="col-xl-8 order-xl-1">
          <card shadow type="secondary">
            <template v-slot:header>
              <div class="bg-white border-0">
                <div class="row align-items-center">
                  <div class="col-8">
                    <h3 class="mb-0">Activity Details</h3>
                  </div>
                </div>
              </div>
            </template>

            <form>
              <h6 class="heading-small text-muted mb-4">Customer Profile</h6>
              <div class="pl-lg-4">
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Name</b>
                    <p>{{cust_profile.name}}</p>
                  </div>
                  <div class="col-lg-6 mb-3">
                    <b>Phone Number</b>
                    <p>{{cust_profile.phone_number}}</p>
                  </div>
                </div>
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Gender</b>
                    <p>{{cust_profile.gender}}</p>
                  </div>
                </div>
              </div>
              <hr class="my-4" />
            </form>

            <form>
              <h6 class="heading-small text-muted mb-4">Activity Info</h6>

              <div class="pl-lg-4">
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Date</b>
                    <p>{{data_act.date}}</p>
                  </div>
                </div>
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Activity Status</b>
                    <br>
                    <div :class="activity_button" >
                      {{ data_act.activity_status }}
                    </div>
                  </div>
                  <div
                    class="col-lg-6 mb-3"
                    v-show="
                      data_act.activity_status != ('finished' || 'cancelled')
                    "
                  >
                    <b>Update Activity Status</b>
                    <base-dropdown>
                      <template v-slot:title>
                        <base-button type="secondary" class="dropdown-toggle">
                          Update Activity Status
                        </base-button>
                      </template>
                      <a
                        class="dropdown-item"
                        @click="updActStatusAction(activity_status[0])"
                        >{{ activity_status[0] }}</a
                      >
                      <a
                        class="dropdown-item"
                        @click="updActStatusAction(activity_status[1])"
                        >{{ activity_status[1] }}</a
                      >
                      <a
                        class="dropdown-item"
                        @click="updActStatusAction(activity_status[2])"
                        >{{ activity_status[2] }}</a
                      >
                      <a
                        class="dropdown-item"
                        @click="updActStatusAction(activity_status[3])"
                        >{{ activity_status[3] }}</a
                      >

                      <a
                        class="dropdown-item"
                        @click="updActStatusAction(activity_status[4])"
                        >{{ activity_status[4] }}</a
                      >
                    </base-dropdown>
                  </div>
                </div>
              </div>
              <div class="row ml-1">
                <div class="col-lg-6 mb-3">
                  <b>Type of Service</b>
                  <p>{{data_act.type_of_service}}</p>
                </div>
                <div class="col-lg-6 mb-3">
                  <b>Price</b>
                  <p>{{data_act.price}}</p>
                </div>
              </div>
              <div class="row ml-1">
                <div class="col-lg-6 mb-3">
                  <b>Start Location</b>
                  <b for="">Latitude</b>
                  <p>{{start_loc.latitude}}</p>
                  <b for="">Longitude</b>
                  <p>{{start_loc.longitude}}</p>
                </div>
                <div class="col-lg-6 mb-3">
                  <b>End Location</b>
                  <b for="">Latitude</b>
                  <p>{{end_loc.latitude}}</p>
                  <b for="">Longitude</b>
                  <p>{{end_loc.longitude}}</p>
                </div>
              </div>
              <div class="row ml-1" v-show="data_act.id_feedback != null">
                <div class="col-lg-6">
                  <p>Feedback</p>
                  <div
                    class="btn btn-info mt-2"
                    @click="feedbackDetailAction(data_act.id_feedback)"
                  >
                    Read Feedback
                  </div>
                </div>
              </div>
              <hr class="my-4" v-show="data_act.type_of_service == 'Antar Barang'" />

              <h6
                class="heading-small text-muted mb-4"
                v-show="data_act.type_of_service == 'Antar Barang'"
              >
                Item Detail
              </h6>

              <div
                class="pl-lg-4"
                v-show="data_act.type_of_service == 'Antar Barang'"
              >
                <div class="row">
                  
                  <div class="col-lg-6 mb-3">
                    <b>Weight</b>
                    <p>{{item_detail.weight}}</p>
                  </div>
                </div>
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Type</b>
                    <p>{{item_detail.type}}</p>
                  </div>
                  <div class="col-lg-6 mb-3">
                    <b>Delivery Instruction</b>
                    <p>{{item_detail.delivery_instruction}}</p>
                  </div>
                </div>
              </div>

              <hr
                class="my-4"
                v-show="data_act.type_of_service == 'Antar Barang'"
              />

              <h6
                class="heading-small text-muted mb-4"
                v-show="data_act.type_of_service == 'Antar Barang'"
              >
                Recipient Details
              </h6>

              <div
                class="pl-lg-4"
                v-show="data_act.type_of_service == 'Antar Barang'"
              >
                <div class="row">
                  <div class="col-lg-6 mb-3">
                    <b>Name</b>
                    <p>{{recipient_detail.recipient_name}}</p>
                  </div>
                  <div class="col-lg-6 mb-3">
                    <b>Phone Number</b>
                    <p>{{recipient_detail.recipient_phone_number}}</p>
                  </div>
                </div>
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

export default {
  name: "user-profile",
  data() {
    return {
      data: "",
      data_act: {},
      start_loc: {},
      end_loc: {},
      item_detail: {},
      recipient_detail: {},
      blockedText: "Block",
      activity_status: [
        "on the way to sender",
        "",
        "",
        "finished",
        "cancelled",
      ],
      activity_button: "",
      cust_profile: {},
    };
  },
  mounted() {
    const url = "/driver/read/activity/" + this.$route.params.id;
    http.get(url).then((response) => {
      this.data_act = response.data[0];
      this.start_loc = response.data[0].start_loc;
      this.end_loc = response.data[0].end_loc;
      if (response.data[0].type_of_service == "Antar Barang") {
        this.item_detail = response.data[0].item_detail;
        this.recipient_detail = response.data[0].recipient_detail;
      }

      if (this.data_act.activity_status == "cancelled") {
        this.activity_button = "btn btn-danger";
      } else if (this.data_act.activity_status == "finished") {
        this.activity_button = "btn btn-success";
      } else if (this.data_act.activity_status == "new order") {
        this.activity_button = "btn btn-default";
      } else {
        this.activity_button = "btn btn-info";
      }

      const url = "/driver/read/cust/" + this.data_act.id_customer;
      http.get(url).then((response) => {
        this.cust_profile = response.data[0].profile;
      });

    });

    if (this.data_act.type_of_service == "Antar Barang") {
      this.activity_status[1] = "item(s) picked up";
      this.activity_status[2] = "on the way to recipient";
    } else {
      this.activity_status[1] = "picked up";
      this.activity_status[2] = "on the way to destination";
    }

    // alert(JSON.stringify(this.data_act.activity_status));
    // alert(this.data_act.id_customer);
  },
  methods: {
    customerDetailAction(_id) {
      this.$router.push({
        name: "Customer Profile (Driver)",
        params: { id: _id },
      });
    },
    feedbackDetailAction(_id) {
      this.$router.push({
        name: "Driver Feedback Detail",
        params: { id: _id },
      });
    },
    updActStatusAction(status) {
      let jsonData = {
        activity_status: status,
      };

      const url = "/driver/update/activitystatus/" + this.$route.params.id;

      http
        .post(url, jsonData)
        .then((response) => {
          if (response.status == 201) {
            this.data_act.activity_status = status;
            alert("Succesfully update activity status");
            if (this.data_act.activity_status == "cancelled") {
              this.activity_button = "btn btn-danger";
            } else if (this.data_act.activity_status == "finished") {
              this.activity_button = "btn btn-success";
            } else if (this.data_act.activity_status == "new order") {
              this.activity_button = "btn btn-default";
            } else {
              this.activity_button = "btn btn-info";
            }
          }
        })
        .catch((error) => {
          alert("Failed to block this driver\n" + error);
        });
    },
  },
};
</script>
<style></style>
