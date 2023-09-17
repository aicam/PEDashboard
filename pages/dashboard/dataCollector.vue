<template>
  <div>
    <v-row
      class="ps-6 pt-5 pb-6"
      style="font-weight: 400; font-size: 28px; color: black; margin-left: 15px"
    >
      Data
    </v-row>
    <!-- Snack -->
    <v-snackbar v-model="snackbar.isOpen" top>
      {{ snackbar.text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar.isOpen = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <!-- Snack -->
    <!-- =================Uploader================== -->
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          {{ title }}
        </v-card-title>

        <v-card-text>
          <v-tabs v-model="tab" dark grow center>
            <v-tab v-for="item in tabs.headers" :key="`M1_${item.tab}`">
              {{ item.tab }}
            </v-tab>
          </v-tabs>

          <v-tabs-items v-model="tab">
            <v-tab-item v-for="item in tabs.headers" :key="`M1_${item.tab}`">
              <v-row>
                <v-col cols="12">
                  <v-file-input
                    v-if="dialogType == 0"
                    v-model="item.uploads"
                    color="deep-purple accent-4"
                    counter
                    label="File input"
                    multiple
                    placeholder="Select your files"
                    prepend-icon="mdi-paperclip"
                    outlined
                    show-size="true"
                  >
                    <template v-slot:selection="{ index, text }">
                      <v-chip
                        v-if="index < 2"
                        color="deep-purple accent-4"
                        dark
                        label
                        small
                      >
                        {{ text }}
                      </v-chip>

                      <span
                        v-else-if="index === 2"
                        class="text-overline grey--text text--darken-3 mx-2"
                      >
                        +{{ files.length - 2 }} File(s)
                      </span>
                    </template>
                  </v-file-input>
                </v-col>
              </v-row>
              <v-row
                cols="12"
                class="w-100 px-3 pt-3 mt-8 justify-space-between"
                style="background: lightgray; border-radius: 5px"
                v-for="fileIndex in item.uploads"
              >
                <p>{{ fileIndex.name }}</p>
                <p>
                  Size:
                  {{ parseFloat(fileIndex.size / 1000000).toFixed(2) }}Mb
                </p>
              </v-row>
              <v-row
                cols="12"
                class="w-100 px-3 pt-3 mt-8 justify-space-between"
                style="background: lightgray; border-radius: 5px"
                v-for="fileIndex in item.sourceCount"
                :key="fileIndex"
                ref="myUploadItems"
              >
                <v-col v-if="dialogType == 1">
                  <p>Company:{{ getRandomCompanyName().name }}</p>
                </v-col>
                <v-col>
                  <p>file-{{ fileIndex }}.pdf</p>
                </v-col>
                <v-col>
                  <p>
                    Size:
                    {{ parseFloat(item.size / item.sourceCount).toFixed(2) }}Tb
                  </p>
                </v-col>
                <v-col cols="2">
                  <v-btn text color="red" @click="hideItem(fileIndex)">
                    <v-icon color="red">mdi-delete</v-icon>
                  </v-btn>
                </v-col>
              </v-row>
            </v-tab-item>
          </v-tabs-items>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- =================Uploader================== -->
    <!-- =================ViewData================== -->
    <v-dialog v-model="viewDataModal.isOpen" width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          {{ viewDataModal.title }}
        </v-card-title>

        <v-card-text v-if="viewDataModal.type == 0">
          <v-row
            cols="12"
            class="w-100 px-3 pt-3 mt-8 justify-space-between"
            style="background: lightgray; border-radius: 5px"
            v-for="company in viewDataModal.dataList"
          >
            <p>{{ company.name }}</p>
          </v-row>
        </v-card-text>
        <v-card-text v-if="viewDataModal.type == 0">
          <v-row
            cols="12"
            class="w-100 px-3 pt-3 mt-8 justify-space-between"
            style="background: lightgray; border-radius: 5px"
            v-for="company in viewDataModal.dataList"
          >
            <p>{{ company.name }}</p>
          </v-row>
        </v-card-text>
        <v-card-text v-else>
          <v-row
            cols="12"
            class="w-100 px-3 pt-3 mt-8 justify-space-between"
            style="background: lightgray; border-radius: 5px"
            v-for="fileIndex in viewDataModal.dataList"
          >
            <p>{{ fileIndex.name }}</p>
            <p>
              Size:
              {{ fileIndex.volume }}
            </p>
          </v-row>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="viewDataModal.isOpen = false">
            close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- =================ViewData================== -->
    <v-container fluid>
      <v-row>
        <v-col cols="12" sm="12" md="12" lg="12" xl="12" xxl="12">
          <v-card class="cart-data">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-database-outline</v-icon>
              <div class="ms-2 my-auto  font-weight-bold" style="width: 100%; text-align: left">
                DATA
              </div>
            </div>
            <v-row style="text-align: center; margin-top: 5px"
              ><p
                style="
                  font-size: 1.3rem;
                  font-weight: 400;
                  width: 100% !important;
                "
              >
                Number of Sources
              </p></v-row
            >
            <div
              class="d-flex flex-row justify-space-between align-center h-100"
              style="margin-top: 10px"
            >
              <div
                class="d-flex flex-column"
                style="
                  width: 100%;
                  border-right-width: 1px;
                  border-right-style: inset;
                  border-right-color: rgba(211, 211, 211, 0.8);
                "
              >
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Public Data
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  40
                </p>
              </div>

              <div
                class="d-flex flex-column"
                style="
                  width: 100%;
                  border-right-width: 1px;
                  border-right-style: inset;
                  border-right-color: rgba(211, 211, 211, 0.8);
                "
              >
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Proprietary Data
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  45
                </p>
              </div>

              <div class="d-flex flex-column" style="width: 100%">
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Target Data
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  7
                </p>
              </div>
            </div>
            <div
              class="mt-8 mb-12"
              style="height: 1px; background: rgba(211, 211, 211, 0.8)"
            ></div>
            <v-row>
              <apexchart
                :options="dataPageTable.dataChartOptions"
                :series="dataPageTable.dataSeries"
                type="bar"
                style="width: 100% !important"
                height="300"
              ></apexchart>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="12" md="6" lg="6" xl="4" xxl="4">
          <v-card>
            <v-app-bar flat color="rgba(0, 0, 0, 0)">
              <v-toolbar-title class="text-h6 pl-0">
                Public Data (75Tb)
              </v-toolbar-title>
              <v-spacer></v-spacer>
            </v-app-bar>

            <v-card-text>
              <v-row>
                <v-col
                  cols="6"
                  sm="12"
                  md="6"
                  lg="6"
                  xl="6"
                  xxl="6"
                  style="padding: 0px 0px 0px 0px"
                >
                  <div
                    class="font-weight-bold ml-8 mb-2"
                    style="text-align: left"
                  >
                    Standard Data
                  </div>

                  <v-timeline align-top dense>
                    <v-timeline-item
                      v-for="message in [
                        {
                          id: 's1567567567',
                          color: 'deep-purple lighten-1',
                          text: '10',
                          title: 'Number of Sources',
                        },
                        {
                          id: 's134534',
                          color: 'deep-purple lighten-1',
                          text: '50Tb',
                          title: 'Data Volume',
                        },
                        {
                          id: 's1234324',
                          color: 'deep-purple lighten-1',
                          text: '90%',
                          title: 'Training Progress',
                        },
                      ]"
                      :key="message.id"
                      :color="message.color"
                      small
                    >
                      <div>
                        <div class="font-weight-normal">
                          <strong>{{ message.title }}</strong>
                          <v-btn
                            text
                            v-if="message.title == 'Number of Sources'"
                            rounded
                            small
                            @click="getNumberOfSources(10, 50, 'Standard Data')"
                          >
                            <v-icon>mdi-eye</v-icon>
                          </v-btn>
                        </div>
                        <div>{{ message.text }}</div>
                      </div>
                    </v-timeline-item>
                  </v-timeline>
                </v-col>
                <v-col
                  cols="6"
                  sm="12"
                  md="6"
                  lg="6"
                  xl="6"
                  xxl="6"
                  style="padding: 0px 0px 0px 0px"
                >
                  <div
                    class="font-weight-bold ml-8 mb-2"
                    style="text-align: left"
                  >
                    Alternative Data
                  </div>

                  <v-timeline align-top dense>
                    <v-timeline-item
                      v-for="message in [
                        {
                          id: 's111',
                          color: 'deep-purple lighten-1',
                          text: '30',
                          title: 'Number of Sources',
                        },
                        {
                          id: 's1112',
                          color: 'deep-purple lighten-1',
                          text: '25Tb',
                          title: 'Data Volume',
                        },

                        {
                          id: 's1113',
                          color: 'deep-purple lighten-1',
                          text: '90%',
                          title: 'Training Progress',
                        },
                      ]"
                      :key="message.id"
                      :color="message.color"
                      small
                    >
                      <div>
                        <div class="font-weight-normal">
                          <strong>{{ message.title }}</strong>
                          <v-btn
                            text
                            v-if="message.title == 'Number of Sources'"
                            rounded
                            small
                            @click="
                              getNumberOfSources(30, 25, 'Alternative Data')
                            "
                          >
                            <v-icon>mdi-eye</v-icon>
                          </v-btn>
                        </div>
                        <div>{{ message.text }}</div>
                      </div>
                    </v-timeline-item>
                  </v-timeline>
                </v-col>
              </v-row>
            </v-card-text>
            <v-card-actions>
              <v-slide-group multiple show-arrows>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="green"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Public Data';
                        tabs.headers = [
                          {
                            tab: 'Standard Data',
                            id: 1,
                            sourceCount: 10,
                            size: 50,
                            uploads: [],
                          },
                          {
                            tab: 'Alternative Data',
                            id: 2,
                            sourceCount: 30,
                            size: 25,
                            uploads: [],
                          },
                        ];
                        dialog = true;
                        dialogType = 1;
                      }
                    "
                    >View</v-btn
                  >
                </v-slide-item>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="primary"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Upload Public Data';
                        tabs.headers = [
                          {
                            tab: 'Standard Data',
                            id: 1,
                            sourceCount: 10,
                            size: 50,
                            uploads: [],
                          },
                          {
                            tab: 'Alternative Data',
                            id: 2,
                            sourceCount: 30,
                            size: 25,
                            uploads: [],
                          },
                        ];

                        dialog = true;
                        dialogType = 0;
                      }
                    "
                    ><v-icon color="primary">mdi-plus</v-icon>Upload</v-btn
                  >
                </v-slide-item>
              </v-slide-group>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col cols="12" sm="12" md="6" lg="6" xl="4" xxl="4">
          <v-card style="height: 100%">
            <v-app-bar flat color="rgba(0, 0, 0, 0)">
              <v-toolbar-title class="text-h6 pl-0">
                Proprietary Data (20Tb)
              </v-toolbar-title>
            </v-app-bar>

            <v-card-text>
              <v-row>
                <v-col
                  cols="6"
                  sm="12"
                  md="6"
                  lg="6"
                  xl="6"
                  xxl="6"
                  style="padding: 0px 0px 0px 0px; height: 100%"
                >
                  <div
                    class="font-weight-bold ml-8 mb-2"
                    style="text-align: left"
                  >
                    Internal Sources
                  </div>
                  <v-timeline align-top dense>
                    <v-timeline-item
                      v-for="message in [
                        {
                          id: 's1112',
                          color: 'deep-purple lighten-1',
                          text: '15',
                          title: 'Number of Sources',
                        },
                        {
                          id: 's11122',
                          color: 'deep-purple lighten-1',
                          text: '8Tb',
                          title: 'Data Volume',
                        },
                        {
                          id: 's11123',
                          color: 'deep-purple lighten-1',
                          text: '60%',
                          title: 'Training Progress',
                        },
                      ]"
                      :key="message.id"
                      :color="message.color"
                      small
                    >
                      <div>
                        <div class="font-weight-normal">
                          <strong>{{ message.title }}</strong>
                          <v-btn
                            text
                            v-if="message.title == 'Number of Sources'"
                            rounded
                            small
                            @click="
                              getNumberOfSources(15, 8, 'Internal Sources')
                            "
                          >
                            <v-icon>mdi-eye</v-icon>
                          </v-btn>
                        </div>
                        <div>{{ message.text }}</div>
                      </div>
                    </v-timeline-item>
                  </v-timeline>
                </v-col>
                <v-col
                  cols="6"
                  sm="12"
                  md="6"
                  lg="6"
                  xl="6"
                  xxl="6"
                  style="padding: 0px 0px 0px 0px"
                >
                  <div
                    class="font-weight-bold ml-8 mb-2"
                    style="text-align: left"
                  >
                    PortCo Sources
                  </div>
                  <v-timeline align-top dense>
                    <v-timeline-item
                      v-for="message in [
                        {
                          id: 's1112',
                          color: 'deep-purple lighten-1',
                          text: '30',
                          title: 'Number of Sources',
                        },
                        {
                          id: 's11122',
                          color: 'deep-purple lighten-1',
                          text: '12Tb',
                          title: 'Data Volume',
                        },
                        {
                          id: 's1',
                          color: 'deep-purple lighten-1',
                          text: '90%',
                          title: 'Training Progress',
                        },
                      ]"
                      :key="message.id"
                      :color="message.color"
                      small
                    >
                      <div>
                        <div class="font-weight-normal">
                          <strong>{{ message.title }}</strong>
                          <v-btn
                            text
                            v-if="message.title == 'Number of Sources'"
                            rounded
                            small
                            @click="
                              getNumberOfSources(30, 12, 'PortCo Sources')
                            "
                          >
                            <v-icon>mdi-eye</v-icon>
                          </v-btn>
                        </div>
                        <div>{{ message.text }}</div>
                      </div>
                    </v-timeline-item>
                  </v-timeline>
                </v-col>
              </v-row>
            </v-card-text>

            <v-card-actions>
              <v-slide-group multiple show-arrows>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="green"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Proprietary Data';
                        tabs.headers = [
                          {
                            tab: 'Internal Sources',
                            id: 1,
                            sourceCount: 15,
                            size: 8,
                            uploads: [],
                          },
                          {
                            tab: 'PortCo Sources',
                            id: 2,
                            sourceCount: 30,
                            size: 12,
                            uploads: [],
                          },
                        ];

                        dialog = true;
                        dialogType = 1;
                      }
                    "
                    >View</v-btn
                  >
                </v-slide-item>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="primary"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Upload Proprietary Data';
                        tabs.headers = [
                          {
                            tab: 'Internal Sources',
                            id: 1,
                            sourceCount: 15,
                            size: 8,
                            uploads: [],
                          },
                          {
                            tab: 'PortCo Sources',
                            id: 2,
                            sourceCount: 30,
                            size: 12,
                            uploads: [],
                          },
                        ];
                        dialog = true;
                        dialogType = 0;
                      }
                    "
                    ><v-icon color="primary">mdi-plus</v-icon>Upload</v-btn
                  >
                </v-slide-item>
              </v-slide-group>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col cols="12" sm="12" md="6" lg="6" xl="4" xxl="4">
          <v-card style="height: 100%">
            <v-app-bar flat color="rgba(0, 0, 0, 0)">
              <v-toolbar-title class="text-h6 pl-0">
                Target Assets (5Tb)
              </v-toolbar-title>
            </v-app-bar>
            <v-card-text>
              <v-row>
                <v-col
                  cols="12"
                  sm="12"
                  md="12"
                  lg="12"
                  xl="12"
                  xxl="12"
                  style="padding: 0px 0px 0px 0px; height: 100%"
                >
                  <v-timeline align-top dense style="height: 100%">
                    <v-timeline-item
                      v-for="message in [
                        {
                          id: 's1112',
                          color: 'deep-purple lighten-1',
                          text: `${allData.companies.length}`,
                          title: 'Number of Companies',
                        },
                        {
                          id: 's11122',
                          color: 'deep-purple lighten-1',
                          text: '5Tb',
                          title: 'Data Volume',
                        },
                        {
                          id: 's1',
                          color: 'deep-purple lighten-1',
                          text: '60%',
                          title: 'Training Progress',
                        },
                      ]"
                      :key="message.id"
                      :color="message.color"
                      small
                    >
                      <div>
                        <div class="font-weight-normal">
                          <strong
                            >{{ message.title }}
                            <v-btn
                              v-if="message.title == 'Number of Companies'"
                              text
                              rounded
                              small
                              @click="getNumberOfCompanies"
                            >
                              <v-icon>mdi-eye</v-icon>
                            </v-btn>
                          </strong>
                        </div>
                        <div>{{ message.text }}</div>
                      </div>
                    </v-timeline-item>
                  </v-timeline>
                </v-col>
              </v-row>
            </v-card-text>
            <v-card-actions>
              <v-slide-group multiple show-arrows>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="green"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Target Assets Data';
                        tabs.headers = [
                          {
                            tab: 'Target Assets',
                            id: 1,
                            sourceCount: 7,
                            size: 5,
                            uploads: [],
                          },
                        ];

                        dialog = true;
                        dialogType = 1;
                      }
                    "
                    >View</v-btn
                  >
                </v-slide-item>
                <v-slide-item style="margin: 7px">
                  <v-btn
                    color="primary"
                    text
                    @click="
                      () => {
                        data = [];
                        title = 'Upload Target Assets Data';
                        tabs.headers = [
                          {
                            tab: 'Target Assets',
                            id: 1,
                            sourceCount: 7,
                            size: 5,
                            uploads: [],
                          },
                        ];
                        dialog = true;
                        dialogType = 0;
                      }
                    "
                    ><v-icon color="primary">mdi-plus</v-icon>Upload</v-btn
                  ></v-slide-item
                >
              </v-slide-group>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import api from "@/components/API";

export default {
  name: "dataCollector",
  components: {
    [process.browser && "apexchart"]: () => import("vue-apexcharts"),
  },
  head() {
    return {
      title: "Data",
    };
  },
  data() {
    return {
      dialog: false,
      dialogType: 0,
      tab: null,
      data: [],
      tabs: {
        headers: [
          { tab: "One", id: 0 },
          { tab: "Two", id: 1 },
          { tab: "Three", id: 2 },
        ],
        files: [
          { name: "file1.pdf", size: "1Tb" },
          { name: "file2.pdf", size: "1Tb" },
          { name: "file3.pdf", size: "1Tb" },
          { name: "file4.pdf", size: "1Tb" },
          { name: "file5.pdf", size: "1Tb" },
          { name: "file6.pdf", size: "1Tb" },
          { name: "file7.pdf", size: "1Tb" },
          { name: "file8.pdf", size: "1Tb" },
        ],
      },
      files: [],
      title: "",
      dataPageTable: {
        dataSeries: [],
        dataChartOptions: {},
      },

      viewDataModal: {
        type: 0,
        title: "",
        isOpen: "",
        dataList: [],
      },
      allData: { companies: [] },
      snackbar: {
        isOpen: false,
        text: "",
      },
    };
  },

  methods: {
    init() {
      this.allData = api.getFullUserData();
      this.fillDataPageTable();
    },
    fillDataPageTable() {
      const dataData = {
        count: 5,
        chartItems: [
          ["Past Data Volume", "75", "55", "5"],
          ["Number of Sources", "40", "45", "7"],
          ["Trained Volume", "95", "75", "97"],
          ["Last Month Data Volume", "70", "30", "22"],
        ],
        chartHeaders: ["", "Public Data", "Proprietary Data", "Target Data"],
      };
      const options = this.getEmptyChartObject();
      options.xaxis.categories = [
        dataData.chartHeaders?.[1],
        dataData.chartHeaders?.[2],
        dataData.chartHeaders?.[3],
      ];
      options.yaxis.labels.formatter = function (val) {
        return val + "Tb";
      };
      options.tooltip.y.formatter = function (val) {
        return val + "Tb";
      };
      const colors = this.getColors();
      options.colors = [colors.primary, colors.info, colors.success];
      this.dataPageTable.dataChartOptions = { ...options };
      this.dataPageTable.dataSeries = [
        {
          name: dataData.chartItems?.[0][0],
          group: "data",
          data: [
            dataData.chartItems?.[0][1],
            dataData.chartItems?.[0][2],
            dataData.chartItems?.[0][3],
          ],
        },
        {
          name: dataData.chartItems?.[3][0],
          group: "data",
          data: [
            dataData.chartItems?.[3][1],
            dataData.chartItems?.[3][2],
            dataData.chartItems?.[3][3],
          ],
        },
        {
          name: dataData.chartItems?.[2][0],
          group: "process",
          data: [
            (
              (parseFloat(dataData.chartItems?.[2][1] || "0") *
                (parseFloat(dataData.chartItems?.[0][1] || "0") +
                  parseFloat(dataData.chartItems?.[3][1] || "0"))) /
              100
            ).toFixed(2),
            (
              (parseFloat(dataData.chartItems?.[2][2] || "0") *
                (parseFloat(dataData.chartItems?.[0][2] || "0") +
                  parseFloat(dataData.chartItems?.[3][2] || "0"))) /
              100
            ).toFixed(2),
            (
              (parseFloat(dataData.chartItems?.[2][3] || "0") *
                (parseFloat(dataData.chartItems?.[0][3] || "0") +
                  parseFloat(dataData.chartItems?.[3][3] || "0"))) /
              100
            ).toFixed(2),
          ],
        },
      ];
    },
    getRandomCompanyName() {
      return this.allData.companies[
        Math.floor(Math.random() * this.allData.companies.length)
      ];
    },
    getNumberOfCompanies() {
      this.allData = api.getFullUserData();
      this.viewDataModal.type = 0;
      this.viewDataModal.title = "Companies";
      this.viewDataModal.dataList = this.allData.companies;
      this.viewDataModal.isOpen = true;
    },
    getNumberOfSources(sourceCount, volume, title) {
      this.allData = api.getFullUserData();
      this.viewDataModal.type = 1;
      this.viewDataModal.title = title;
      this.viewDataModal.dataList = [];
      for (let index = 0; index < sourceCount; index++) {
        this.viewDataModal.dataList.push({
          name: `${title}-${index}`,
          volume: `${parseFloat(volume / sourceCount).toFixed(2)}Tb`,
        });
      }
      this.viewDataModal.isOpen = true;
    },
    fireSnack(text) {
      this.snackbar.text = text;
      this.snackbar.isOpen = true;
    },
    hideItem(i) {
      this.$refs.myUploadItems[i].style["display"] = "none";
      this.fireSnack("item removed successfully");
    },
    getColors() {
      return {
        primary: "#8b5cf6",
        info: "#0ea5e9",
        success: "#14b8a6",
        warning: "#d97706",
      };
    },
    getEmptyChartObject() {
      return {
        chart: {
          type: "bar",
          height: 350,
          stacked: true,
          toolbar: {
            show: false,
          },
        },
        colors: ["#80c7fd", "#008FFB", "#80f1cb", "#00E396"],
        legend: {
          position: "top",
          horizontalAlign: "center",
        },
        stroke: {
          show: true,
          width: 2,
          colors: ["transparent"],
        },
        dataLabels: {
          enabled: false,
        },
        plotOptions: {
          bar: {
            horizontal: false,
            endingShape: "rounded",
            columnWidth: "55%",
          },
        },
        xaxis: {
          categories: [],
        },
        yaxis: {
          labels: {
            formatter: function (val) {
              return val;
            },
          },
        },
        fill: {
          opacity: 1,
        },
        tooltip: {
          y: {
            formatter: function (val) {
              return val;
            },
          },
        },
      };
    },
  },
  mounted() {
    if (!api.getAuth()) window.location.href = "/login";

    this.init();
  },
  watch: {
    tab(newVal) {
      console.log(newVal);
    },
  },
};
</script>

<style>
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
.cart-data {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(255, 206, 143) 100%
  );
}
</style>
