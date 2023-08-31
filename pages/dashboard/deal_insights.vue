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
            <v-btn class=" d-flex flex-row mx-auto mt-6" style="background: black; color: white; border-radius: 30px; text-transform: capitalize">View Details</v-btn>
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
            <v-btn class=" d-flex flex-row mx-auto mt-6" style="background: black; color: white; border-radius: 30px; text-transform: capitalize">View Details</v-btn>
          </v-col>
        </v-card>
      </v-col>
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
          <v-btn class=" d-flex flex-row ms-auto me-2 mt-2" style="background: black; color: white; border-radius: 30px; text-transform: capitalize">View</v-btn>
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
    </v-row>

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

    <!-- Focus search Dialog START -->
    <v-dialog
      v-model="dialogAddFocusSearch"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500">Please filter the search based on your KPIs. By clicking on '+' you can add an AI based custom KPI</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-col>
              <v-btn
                style="border-radius: 20px; background: black; color: white"
                @click="dialogSelectFromSearchHistory = true"
              >
                Select from search history
              </v-btn>
              <v-row class="mt-5">
                <p class="mt-3 me-3">Label name</p>
                <v-text-field
                  outlined
                  label="Input value">
                </v-text-field>
              </v-row>
              <v-row class="mt-5">
                <p class="mt-1 me-3">Label name</p>
                <v-slider
                  :thumb-size="28"
                  color="black"
                  thumb-label="always"
                ></v-slider>
              </v-row>

              <v-row class="mt-5 px-3 py-3" style="background: lightskyblue; border-radius: 7px">
                <p class="mt-3 me-3">Custom KPI</p>
                <v-text-field
                  outlined
                  label="Input value">
                </v-text-field>
                <v-icon @click="() => console.log('delete custom dialog')" class="mt-0 mb-auto ms-4 pointer" style="font-size: 20px; color: white; background: red; border-radius: 50%">mdi-close</v-icon>
              </v-row>

              <v-btn
                @click="dialogAddNewAIBasedKPI = true"
                rounded
                color="gray"
                class="mt-10 mx-auto"
                style="color: black"
              >
                <v-icon>mdi-plus</v-icon>
                Add new AI based KPI
              </v-btn>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogAddFocusSearch = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="dialogSearchFilterName = true"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Focus search Dialog END -->

    <!-- Select From Search History Dialog START -->
    <v-dialog
      v-model="dialogSelectFromSearchHistory"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500">Searches history</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-col>
              <v-card>
                <v-col class="p-3">
                  <v-text-field
                    outlined
                    label="Search name"
                  ></v-text-field>
                  <v-textarea
                    outlined
                    label="Search description"
                  ></v-textarea>
                  <v-spacer></v-spacer>
                  <v-row justify="between">
                    <v-btn
                      color="red darken-1"
                      text
                      @click="dialogSelectFromSearchHistory = false"
                    >
                      Delete
                    </v-btn>
                    <v-btn
                      color="green darken-1"
                      text
                      @click="() => console.log('')"
                    >
                      Select
                    </v-btn>
                  </v-row>
                </v-col>
              </v-card>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogAddFocusSearch = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('save focus search')"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Select From Search History Dialog END -->

    <!-- Add New AI based KPI Dialog START -->
    <v-dialog
      v-model="dialogAddNewAIBasedKPI"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500" @click="dialogAddNewAIBasedKPI = true">Add new AI based KPI</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-col>
              <v-card>
                <v-col class="p-3">
                  <v-text-field
                    outlined
                    label="KPI label"
                  ></v-text-field>
                  <v-text-field
                    outlined
                    label="KPI Type"
                  ></v-text-field>

                  <v-spacer></v-spacer>
                  <p>Please write your KPI logic for this input here and AI model will filters the data based on your topic</p>
                  <v-textarea
                    outlined
                    label="KPI logic"
                  ></v-textarea>
                  <v-spacer></v-spacer>
                  <v-row justify="between">
                    <v-btn
                      color="red darken-1"
                      text
                      @click="dialogSelectFromSearchHistory = false"
                    >
                      Delete
                    </v-btn>
                    <v-btn
                      color="green darken-1"
                      text
                      @click="() => console.log('')"
                    >
                      Select
                    </v-btn>
                  </v-row>
                </v-col>
              </v-card>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogAddNewAIBasedKPI = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('save focus search')"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Add New AI based KPI Dialog END -->

    <!-- Search Filter Name Dialog START -->
    <v-dialog
      v-model="dialogSearchFilterName"
      max-width="500px"
    >
      <v-card>
        <v-card-title>
          <span class="px-3" style="font-size: 18px; font-weight: 500" @click="dialogAddNewAIBasedKPI = true">Please enter name of your search filter</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-col>
              <v-card>
                <v-col class="p-3">
                  <v-text-field
                    outlined
                    label="Search filter name"
                  ></v-text-field>
                  <v-spacer></v-spacer>
                </v-col>
              </v-card>
            </v-col>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="dialogSearchFilterName = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="() => console.log('name for your search filter')"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Search Filter Name Dialog END -->
  </div>
</template>

<script>
import utils from "@/components/utils";

export default {
  name: 'Deal Insights page',
  data () {
    return {
      expanded: [],
      dialogAddReview: false,
      dialogAddFocusSearch: false,
      dialogSelectFromSearchHistory: false,
      dialogAddNewAIBasedKPI: false,
      dialogSearchFilterName: false,
      headers: [
        {
          text: 'Name',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Score', value: 'score' },
        { text: 'HQ Location', value: 'hqLocation' },
        { text: 'Revenue Size', value: 'revenueSize' },
        { text: 'Industry', value: 'industry' },
        { text: 'Insight', value: 'data-table-expand' },
      ],
      allCompanies: [],
      companies: null,
      precisionVsRecall: 45,
      currentSearches: [
        {
          label: "0",
          value: "All"
        },
        {
          label: "1",
          value: "Apple"
        },
        {
          label: "2",
          value: "BMW"
        },
        {
          label: "3",
          value: "SpaceX"
        },
        {
          label: "4",
          value: "Tesla"
        },
        {
          label: "5",
          value: "Benz"
        },
        {
          label: "6",
          value: "Coca Cola"
        },
        {
          label: "7",
          value: "Toronto, Ontario, Canada"
        },
        {
          label: "8",
          value: "Los Angeles"
        },
        {
          label: "9",
          value: "Audi"
        },
        {
          label: "10",
          value: "Ottawa"
        },
        {
          label: "11",
          value: "Walmart"
        },
        {
          label: "12",
          value: "New York, USA"
        },
        {
          label: "13",
          value: "BMW"
        },
        {
          label: "14",
          value: "SpaceX"
        },
        {
          label: "15",
          value: "Tesla"
        },
        {
          label: "16",
          value: "Benz"
        },
        {
          label: "17",
          value: "Coca Cola"
        },
        {
          label: "18",
          value: "Toronto, Ontario, Canada"
        },
      ]
    }
  },
  methods: {
    getRandomColor(colorArray) {
      const randomIndex = Math.floor(Math.random() * colorArray.length);
      return colorArray[randomIndex];
    },
    setSelectForInsight(item) {
      let selectedForInsights
      if (localStorage.getItem("selectedForInsights")) {
        selectedForInsights = JSON.parse(localStorage.getItem("selectedForInsights"))
        if (!selectedForInsights.filter((itemClone) => itemClone === item).length) {
          selectedForInsights.push(item)
        }
      } else {
        selectedForInsights = [item]
      }
      localStorage.setItem("selectedForInsights", JSON.stringify(selectedForInsights))
    },
    setStartForTracking(item) {
      let startedForTracking
      if (localStorage.getItem("startedForTracking")) {
        startedForTracking = JSON.parse(localStorage.getItem("startedForTracking"))
        if (!startedForTracking.filter((itemClone) => itemClone === item).length) {
          startedForTracking.push(item)
          localStorage.setItem("startedForTracking", JSON.stringify(startedForTracking))
        }
      } else {
        startedForTracking = [item]
        localStorage.setItem("startedForTracking", JSON.stringify(startedForTracking))
      }
    },
  },
  mounted() {
    this.allCompanies = utils.getCompanies()
    this.companies = this.allCompanies
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
