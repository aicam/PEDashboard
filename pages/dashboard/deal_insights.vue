<template>
  <div>
    <v-row class="ps-3 pt-8 pb-6 mb-3" style="font-weight: 600; font-size: 28px; background: black; color: white">Deal Insights</v-row>
    <div class="d-flex flex-row justify-end">
      <v-btn
        @click="dialogAddNewAIBasedKPI = true"
        rounded
        color="black"
        class="mt-4"
        style="color: white;"
      >
        <v-icon>mdi-plus</v-icon>
        Add asset for insight
      </v-btn>
    </div>
    <v-row style="justify-content: space-between">
      <v-col cols="12" sm="6" md="3" lg="3" xl="3" xxl="3">
        <v-card
          style="
          padding-top: 10px;
          padding-bottom: 2px;
          padding-left: 10px;
          padding-right: 10px;
          background: linear-gradient(82.99deg, rgb(92, 105, 226) 5.47%, rgb(8, 207, 234) 94.53%);
          color: white;
         "
        >
          <v-col class="justify-space-between align-center h-100">
            <p class="text-center" style="font-size: 2rem; line-height: 2.75rem; font-weight: 700">{{`5`}}</p>
            <p class="text-center" style="font-size: 1.2rem; line-height: 1.5rem; font-weight: 700">Company insights</p>
            <div style="height: 2px; background: red"></div>
            <div class="candlestick">
              <div class="wick"></div>
              <div class="body" style="width: 70%; left: 10%"></div>
              <div class="shadow-left" style="width: 10%"></div>
              <div class="shadow-right" style="width: 20%"></div>
            </div>
            <v-row class="justify-space-between mt-1 px-2">
              <p style="font-size: 10px">min: 10%</p>
              <p style="font-size: 10px">max: 70%</p>
            </v-row>
          </v-col>
        </v-card>
      </v-col>
      <v-col class="mt-7" cols="12" sm="6" md="9" lg="9" xl="9" xxl="9">
        <p class="mt-6" style="font-size: 16px; font-weight: 500">Sample Text for deal insights ...</p>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="6" md="4" lg="4" xl="3" xxl="3">
        <v-card
          style="
          padding: 10px;
          background: #F0F0F0;
          color: black;
        "
        >
          <v-col class="justify-space-between align-center h-100">
            <div class="d-flex flex-row justify-end">
              <v-icon style="color: red" @click="dialogDeleteAsset = true">mdi-close</v-icon>
            </div>
            <p class="text-center" style="font-size: 2rem; line-height: 2.75rem; font-weight: 700">{{`Apple`}}</p>
            <v-row class="mt-3">
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total Gathered data: 30 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total uploaded documents: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Score: 83`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Completeness: 83`}}</p>
            </v-row>
            <v-btn class=" d-flex flex-row mx-auto mt-6" @click="dialogViewAssetDetail = true" style="background: black; color: white; border-radius: 30px; text-transform: capitalize">View Details</v-btn>
          </v-col>
        </v-card>
      </v-col>
      <v-col cols="12" sm="6" md="4" lg="4" xl="3" xxl="3">
        <v-card
          style="
          padding: 10px;
          background: #F0F0F0;
          color: black;
        "
        >
          <v-col class="justify-space-between align-center h-100">
            <div class="d-flex flex-row justify-end">
              <v-icon style="color: red" @click="dialogDeleteAsset = true">mdi-close</v-icon>
            </div>
            <p class="text-center" style="font-size: 2rem; line-height: 2.75rem; font-weight: 700">{{`Apple`}}</p>
            <v-row class="mt-3">
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total Gathered data: 30 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total uploaded documents: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Score: 83`}}</p>
            </v-row>
            <v-row>
              <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
              <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Completeness: 83`}}</p>
            </v-row>
            <v-btn class=" d-flex flex-row mx-auto mt-6" @click="dialogViewAssetDetail = true" style="background: black; color: white; border-radius: 30px; text-transform: capitalize">View Details</v-btn>
          </v-col>
        </v-card>
      </v-col>
    </v-row>

    <!-- Delete asset Dialog START -->
    <v-dialog
      v-model="dialogDeleteAsset"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500">Are you sure want to delete this asset</span>
        </v-card-title>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogDeleteAsset = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('delete asset')"
          >
            Yes
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Delete asset Dialog END -->

    <!-- View Asset Detail Dialog START -->
    <v-dialog
      v-model="dialogViewAssetDetail"
      max-width="90vw"
    >
      <v-card class="mt-5">
        <v-card-text>
          <v-container>
            <v-col>
              <v-row style="border-bottom: 1px solid gray">
                <v-col cols="12" sm="12" md="6" lg="6" xl="6" xxl="6">
                  <v-card
                    style="
                    padding: 10px;
                    background: #F0F0F0;
                    color: black;
                  "
                  >
                    <v-col class="justify-space-between align-center h-100">
                      <p class="text-center" style="font-size: 2rem; line-height: 2.75rem; font-weight: 700">{{`Apple`}}</p>
                      <v-row class="mt-3">
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
                      </v-row>
                      <v-row>
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total Gathered data: 30 Gb`}}</p>
                      </v-row>
                      <v-row>
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total uploaded documents: 26 Gb`}}</p>
                      </v-row>
                      <v-row>
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Total trained data: 26 Gb`}}</p>
                      </v-row>
                      <v-row>
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Score: 83`}}</p>
                      </v-row>
                      <v-row>
                        <img alt="logo" class="ps-3 pe-2" style="width: 32px; height: 32px" src="../../static/vuetify-logo.svg">
                        <p class="text-center" style="font-size: 0.7rem; line-height: 1.5rem; font-weight: 700">{{`Completeness: 83`}}</p>
                      </v-row>
                    </v-col>
                  </v-card>
                  <v-row class="justify-space-between mt-3">
                    <v-chip
                      class="ma-2"
                      color="gray"
                      label
                      text-color="black"
                    >
                      <v-icon left>
                        mdi-information
                      </v-icon>
                      BioTech
                    </v-chip>
                    <v-chip
                      class="ma-2"
                      color="#F0F0F0"
                      label
                      text-color="black"
                    >
                      <v-icon left>
                        mdi-puzzle
                      </v-icon>
                      Technology, Medicare, Wearable devices
                    </v-chip>
                  </v-row>
                </v-col>
                <v-col cols="12" sm="12" md="6" lg="6" xl="6" xxl="6">
                  <v-row class="mt-1 justify-end">
                    <v-btn class="me-2" color="black" style="color: white; border-radius: 20px">Chat with AI</v-btn>
                    <v-btn color="white" style="color: black; border-radius: 20px" @click="dialogMonitoring = true">Monitoring</v-btn>
                  </v-row>
                  <v-row class="w-100 mt-3 justify-center">
                    <v-btn class="w-100 mt-5 me-2" color="gray" style="color: black; border-radius: 20px">
                      <v-icon>mdi-plus</v-icon>
                      Upload more documents
                    </v-btn>
                  </v-row>
                  <v-row class="w-100 px-3 pt-3 mt-8 justify-space-between" style="background: lightgray; border-radius: 5px">
                    <p>File Name.pdf</p>
                    <p>Size: 34Kb</p>
                    <v-icon class="mt-0 mb-auto" style="color: red">mdi-delete</v-icon>
                  </v-row>
                  <v-row class="w-100 px-3 pt-3 mt-8 justify-space-between" style="background: lightgray; border-radius: 5px">
                    <p>File Name.pdf</p>
                    <p>Size: 34Kb</p>
                    <v-icon class="mt-0 mb-auto" style="color: red">mdi-delete</v-icon>
                  </v-row>
                  <v-row class="w-100 px-3 pt-3 mt-8 justify-space-between" style="background: lightgray; border-radius: 5px">
                    <p>File Name.pdf</p>
                    <p>Size: 34Kb</p>
                    <v-icon class="mt-0 mb-auto" style="color: red">mdi-delete</v-icon>
                  </v-row>

                </v-col>
              </v-row>
              <v-row class="my-5 justify-center">
                <v-btn class="mt-3 me-2" color="black" style="color: white; border-radius: 20px">Standard Due Diligence</v-btn>
                <v-btn class="mt-3" color="white" style="color: black; border-radius: 20px">Alternative Insights</v-btn>
              </v-row>
              <v-card style="max-height: 90vh">
                <v-card-text>
                  <v-container>
                    <v-row class="mb-5 justify-center">
                      <v-btn class="mt-3 me-2" color="black" style="color: white; border-radius: 20px">Commercial</v-btn>
                      <v-btn class="mt-3 me-2" color="white" style="color: black; border-radius: 20px">Finance</v-btn>
                      <v-btn class="mt-3 me-2" color="black" style="color: white; border-radius: 20px">Legal</v-btn>
                      <v-btn class="mt-3" color="white" style="color: black; border-radius: 20px">IT</v-btn>
                    </v-row>
                    <p style="max-height: 50vh">
                      Sample Text for this. Sample Text for this. Sample Text for this. Sample Text for this.Sample Text for this. Sample Text for this.
                      Sample Text for this. Sample Text for this. Sample Text for this. Sample Text for this.Sample Text for this. Sample Text for this.
                      Sample Text for this. Sample Text for this. Sample Text for this. Sample Text for this.Sample Text for this. Sample Text for this.
                      Sample Text for this. Sample Text for this. Sample Text for this. Sample Text for this.Sample Text for this. Sample Text for this.
                      Sample Text for this. Sample Text for this. Sample Text for this. Sample Text for this.Sample Text for this. Sample Text for this.
                    </p>
                  </v-container>
                </v-card-text>
              </v-card>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogViewAssetDetail = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- View Asset Detail Dialog END -->

    <!-- View Asset Detail Dialog START -->
    <v-dialog
      v-model="dialogMonitoring"
      max-width="90vw"
    >
      <v-card>
        <v-card-text>
          <v-container>
            <v-col class="py-10">
              <v-row class="px-3">
                <p style="font-size: 20px; font-weight: 600;" class="my-3">Insight</p>
                <div class="d-flex flex-row ms-auto me-0 my-3">
                  <v-row>
                    <v-btn
                      @click="dialogAddReview = true"
                      rounded
                      color="gray"
                      class="mt-4 mx-3"
                      style="color: black"
                    >
                      <v-icon>mdi-plus</v-icon>
                      Add a review
                    </v-btn>
                  </v-row>
                </div>
              </v-row>
              <p style="font-size: 14px; font-weight: 400" class="mt-10">Sample Text ...</p>
              <v-row class="d-flex justify-space-between mt-10">
                <p style="font-size: 14px; font-weight: 400" class="mt-4 ps-3">
                  {{`There is 2.3 Gb of data gathered for this company`}}
                </p>
              </v-row>
              <v-card class="mt-10" style="background: #F0F0F0">
                <v-row style="justify-content: center; align-items: center">
                  <img alt="logo" class="ps-4" style="width: 40px; height: 40px" src="../../static/vuetify-logo.svg">
                  <v-col>
                    <v-chip
                      class="ma-2"
                      color="green"
                      text-color="white"
                      style="justify-content: center; align-items: center"
                    >
                      {{`Status: Gathering Data`}}
                    </v-chip>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`Last Update: 12.02.2023`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`Gathered Data: 30 Mb`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`AI Model: Some Model`}}
                      </v-chip>
                    </v-row>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="cyan"
                        text-color="white"
                      >
                        {{`Data Gathering Process: 90%`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="cyan"
                        text-color="white"
                      >
                        {{`Data Gathering remaining time: 59 minutes`}}
                      </v-chip>
                    </v-row>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="orange"
                        text-color="white"
                      >
                        {{`AI Learning Process: 0%`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="orange"
                        text-color="white"
                      >
                        {{`AI Learning remaining time: 1h:59m:12s`}}
                      </v-chip>
                    </v-row>
                  </v-col>
                </v-row>
              </v-card>
              <v-card class="mt-10" style="background: #F0F0F0">
                <v-row style="justify-content: center; align-items: center">
                  <img alt="logo" class="ps-4" style="width: 40px; height: 40px" src="../../static/vuetify-logo.svg">
                  <v-col>
                    <v-chip
                      class="ma-2"
                      color="green"
                      text-color="white"
                      style="justify-content: center; align-items: center"
                    >
                      {{`Status: Gathering Data`}}
                    </v-chip>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`Last Update: 12.02.2023`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`Gathered Data: 30 Mb`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="#87CEEB"
                        text-color="white"
                      >
                        {{`AI Model: Some Model`}}
                      </v-chip>
                    </v-row>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="cyan"
                        text-color="white"
                      >
                        {{`Data Gathering Process: 90%`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="cyan"
                        text-color="white"
                      >
                        {{`Data Gathering remaining time: 59 minutes`}}
                      </v-chip>
                    </v-row>
                    <v-row class="ps-4 pt-3">
                      <v-chip
                        class="ma-2"
                        color="orange"
                        text-color="white"
                      >
                        {{`AI Learning Process: 0%`}}
                      </v-chip>
                      <v-chip
                        class="ma-2"
                        color="orange"
                        text-color="white"
                      >
                        {{`AI Learning remaining time: 1h:59m:12s`}}
                      </v-chip>
                    </v-row>
                  </v-col>
                </v-row>
              </v-card>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogMonitoring = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('')"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- View Asset Detail Dialog END -->

    <!-- Add a review Dialog START -->
    <v-dialog
      v-model="dialogAddReview"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500">Please leave your review about the trained data</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="12"
                md="12"
              >
                <v-textarea
                  outlined
                  label="type your review here"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogAddReview = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('save review')"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Add a review Dialog END -->
  </div>
</template>

<script>
import utils from "@/components/utils";

export default {
  name: 'Deal Insights page',
  data () {
    return {
      expanded: [],
      dialogDeleteAsset: false,
      dialogViewAssetDetail: false,
      dialogMonitoring: false,
      dialogAddReview: false,
    }
  },
  methods: {
  },
  mounted() {
  },
  watch: {
  }
}
</script>

<style>
.circleStyle {
  width: 20px;
  height: 20px;
  background-color: red;
  margin-right: 10px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 12px;
}

.candlestick {
  width: 90%;
  height: 20px;
  margin-left: auto;
  margin-right: auto;
  position: relative;
  margin-top: 20px;
}

.body {
  /*width: 70%;*/
  height: 14px;
  background-color: rgba(0, 0, 255, 0.6);
  border-radius: 3px;
  position: absolute;
  /*left: 10%;*/
  top: 3px;
}

.shadow-left {
  /*width: 10%;*/
  height: 3px;
  background-color: rgba(255, 0, 0, 0.6);
  position: absolute;
  left: 0;
  top: 8px;
}

.shadow-right {
  /*width: 20%;*/
  height: 3px;
  background-color: rgba(255, 0, 0, 0.6);
  position: absolute;
  right: 0;
  top: 8px;
}
</style>
