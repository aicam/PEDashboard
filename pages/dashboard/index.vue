<template>
  <v-row style="background: rgba(199, 184, 255, 0.1); padding: 0px">
    <v-row
      class="ps-6 pt-5 pb-6"
      style="
        font-weight: 400;
        font-size: 28px;
        color: black;
        margin-left: 15px;
        margin-right: 15px;
      "
    >
      <v-col cols="12" sm="12" md="8" lg="8" xl="9" xxl="10" class="">
        Dashboard
      </v-col>
      <v-col cols="12" sm="12" md="4" lg="4" xl="3" xxl="2">
        <div class="d-flex flex-row pe-2" style="justify-content: flex-end">
          <v-btn
            color="white"
            class="primary--text d-flex"
            block
            @click="
              () => {
                chatModal.isOpen = true;
                chatModal.situation = 0;
                chatModal.userCommand = '';
                chatModal.messages = [
                  {
                    time: new Date(),
                    from: 'robot',
                    message:
                      'Hello im your AI assistant, please describe me what kind of information do you want about this Page',
                    hasBTN: false,
                  },
                ];
              }
            "
            style="
              text-transform: capitalize;
              height: 40px;
              border-radius: 12px;
              margin-top: 10px;
            "
          >
            <v-icon right dark class="me-4"> mdi-robot-outline </v-icon>
            Talk with AI
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <!-- Chat Dialog START -->
    <v-dialog v-model="chatModal.isOpen" max-width="90vw">
      <v-card>
        <v-card-text>
          <v-container>
            <v-row justify="space-around d-flex flex-column">
              <v-card
                v-for="message in chatModal.messages"
                :key="message.time"
                flat
              >
                <v-list-item
                  :key="message.time"
                  v-if="message.from != 'You'"
                  class=""
                >
                  <v-list-item-avatar class="align-self-start mr-2">
                    <v-avatar size="40">
                      <v-img src="https://via.placeholder.com/50"></v-img>
                    </v-avatar>
                  </v-list-item-avatar>
                  <v-list-item-content class="received-message">
                    <v-card color="green darken-1" class="flex-none">
                      <v-card-text class="white--text pa-2 d-flex flex-column">
                        <span class="text-caption">{{ message.from }} </span>
                        <span class="align-self-start text-subtitle-1">{{
                          message.message
                        }}</span>
                        <span class="text-caption font-italic align-self-end">{{
                          message.time
                        }}</span>

                        <span
                          v-if="message.hasBTN"
                          style="width: 100%; margin-top: 20px"
                          class="align-self-start text-subtitle-1"
                          ><hr />
                          Did you satisfied out of this answer?</span
                        >
                      </v-card-text>
                      <v-card-actions v-if="message.hasBTN">
                        <v-spacer></v-spacer>
                        <v-btn
                          color="red"
                          @click="
                            () => {
                              chatModal.situation = -1;
                              chatWithAIBtnSubmitted();
                            }
                          "
                        >
                          No
                        </v-btn>
                        <v-btn
                          color="green"
                          @click="
                            () => {
                              chatModal.situation = 1;
                              chatWithAIBtnSubmitted();
                            }
                          "
                        >
                          Yes
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item v-else :key="message.time + '1'">
                  <v-list-item-content class="sent-message justify-end">
                    <v-card color="primary" class="flex-none">
                      <v-card-text class="white--text pa-2 d-flex flex-column">
                        <span class="text-subtitle-1 chat-message">{{
                          message.message
                        }}</span>
                        <span
                          class="text-caption font-italic align-self-start"
                          >{{ message.time.toUTCString() }}</span
                        >
                      </v-card-text>
                    </v-card>
                  </v-list-item-content>
                  <v-list-item-avatar class="align-self-start ml-2">
                    <v-img src="https://via.placeholder.com/50"></v-img>
                  </v-list-item-avatar>
                </v-list-item>
              </v-card>
            </v-row>
            <v-row>
              <v-text-field
                v-model="chatModal.userCommand"
                :disabled="chatModal.isCommandInputDisabled"
                :messages="['Ask me anything']"
              >
                <v-btn
                  slot="append"
                  text
                  @click="chatWithAIBtnSubmitted"
                  :disabled="chatModal.isCommandInputDisabled"
                >
                  <v-icon color="primary"> mdi-send </v-icon>
                </v-btn>
              </v-text-field>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="
              () => {
                chatModal.isOpen = false;
                chatModal.situation = 0;
                chatModal.userCommand = '';
                chatModal.messages = [
                  {
                    time: new Date(),
                    from: 'robot',
                    message:
                      'Hello im your AI assistant, please describe me what kind of information do you want about this company',
                    hasBTN: false,
                  },
                ];
              }
            "
          >
            close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- Chat Asset Detail Dialog END -->
    <v-container fluid>
      <v-row style="margin-left: 10px">
        <v-col cols="12" sm="6" md="6" lg="6" xl="6" xxl="6">
          <v-card class="cart-deals">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-shield-check</v-icon>
              <div class="my-auto ms-2 font-weight-bold">RECOMMENDATIONS</div>
              <v-btn
                class="d-flex flex-row ms-auto me-2 my-auto"
                style="
                  background: white;
                  color: black;
                  border: 1px solid #cccccc;
                  border-radius: 15px;
                  text-transform: capitalize;
                "
                @click="redirect('/dashboard/recommendations')"
                >View</v-btn
              >
            </div>

            <div
              class="d-flex flex-row justify-space-between align-center h-100"
              style="margin-top: 50px"
            >
              <div class="d-flex flex-column mx-auto">
                <p
                  class="text-center"
                  style="
                    font-size: 1.8rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{ summaryRecommendedCompanies.count }}
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Proprietary Deals Found
                </p>
              </div>
              <div
                style="
                  width: 1px;
                  height: 100px;
                  background: rgba(211, 211, 211, 0.8);
                "
              ></div>
              <div class="d-flex flex-column mx-auto">
                <p
                  class="text-center"
                  style="
                    font-size: 1.8rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{ summaryRecommendedCompanies.median }}
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Median Score
                </p>
              </div>
            </div>
          </v-card>
        </v-col>
        <v-col cols="12" sm="6" md="6" lg="6" xl="6" xxl="6">
          <v-card class="cart-track">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-radar</v-icon>
              <div class="ms-2 my-auto font-weight-bold">TRACKED ASSETS</div>
              <v-btn
                class="d-flex flex-row ms-auto me-2 my-auto"
                style="
                  background: white;
                  color: black;
                  border: 1px solid #cccccc;
                  border-radius: 15px;
                  text-transform: capitalize;
                "
                @click="redirect('/dashboard/tracking')"
                >View</v-btn
              >
            </div>

            <div
              class="d-flex flex-row justify-space-between align-center h-100"
              style="margin-top: 50px"
            >
              <div class="d-flex flex-column mx-auto">
                <p
                  class="text-center"
                  style="
                    font-size: 1.8rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{ allTrackingSelectedCompanies.length }}
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Assets Tracked
                </p>
              </div>
              <div
                style="
                  width: 1px;
                  height: 100px;
                  background: rgba(211, 211, 211, 0.8);
                "
              ></div>
              <div class="d-flex flex-column mx-auto">
                <p
                  class="text-center"
                  style="
                    font-size: 1.8rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{
                    allTrackingSelectedCompanies.filter((x) => x.readyToBuy)
                      .length
                  }}
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 1.5rem;
                    font-weight: 400;
                  "
                >
                  Ready to Buy
                </p>
              </div>
            </div>
          </v-card>
        </v-col>
      </v-row>
      <v-col
        style="margin-left: 10px"
        cols="12"
        sm="12"
        md="12"
        lg="12"
        xl="12"
        xxl="12"
      >
        <v-card class="cart-insights">
          <div
            class="d-flex flex-row mx-auto"
            style="justify-content: space-between; align-items: center"
          >
            <v-icon large>mdi-head-lightbulb</v-icon>
            <div class="ms-2 my-auto font-weight-bold">INSIGHTS</div>
            <v-btn
              class="d-flex flex-row ms-auto me-2 my-auto"
              style="
                background: white;
                color: black;
                border: 1px solid #cccccc;
                border-radius: 15px;
                text-transform: capitalize;
              "
              @click="redirect('/dashboard/deal_insights')"
              >View</v-btn
            >
          </div>

          <div
            class="d-flex flex-row justify-center align-center h-100"
            style="margin-top: 50px"
          >
            <div
              class="d-flex flex-column"
              style="font-size: 1.3rem; font-weight: 400"
            >
              {{ `Total Assets: ${allInsightCompanies.length}` }}
            </div>
          </div>
          <div
            class="mt-8 mb-12"
            style="height: 1px; background: rgba(211, 211, 211, 0.8)"
          ></div>
          <v-row>
            <v-col cols="12" sm="12" md="12" lg="6" xl="6" xxl="6">
              <apexchart
                :options="insightTable.dataChartOptions"
                :series="insightTable.dataSeries"
                type="bar"
                style="width: 100% !important"
                height="300px"
              ></apexchart>
            </v-col>
            <v-col cols="12" sm="12" md="12" lg="6" xl="6" xxl="6">
              <v-row style="text-align: center; width: 100%">
                <span class="font-weight-regular" style="margin: 0 auto"
                  >Progress completeness</span
                >
              </v-row>
              <v-row>
                <v-col
                  cols="12"
                  sm="4"
                  md="4"
                  lg="4"
                  xl="4"
                  xxl="4"
                  style="padding: 0px"
                >
                  <apexchart
                    :options="insightTable.p0.dataChartOptions"
                    :series="insightTable.p0.dataSeries"
                    style="width: 100% !important"
                    height="250"
                  ></apexchart>
                </v-col>
                <v-col
                  cols="12"
                  sm="4"
                  md="4"
                  lg="4"
                  xl="4"
                  xxl="4"
                  style="padding: 0px"
                >
                  <apexchart
                    :options="insightTable.p1.dataChartOptions"
                    :series="insightTable.p1.dataSeries"
                    style="width: 100% !important"
                    height="250"
                  ></apexchart>
                </v-col>
                <v-col
                  cols="12"
                  sm="4"
                  md="4"
                  lg="4"
                  xl="4"
                  xxl="4"
                  style="padding: 0px"
                >
                  <apexchart
                    :options="insightTable.p2.dataChartOptions"
                    :series="insightTable.p2.dataSeries"
                    style="width: 100% !important"
                    height="250"
                  ></apexchart>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-row style="margin-left: 10px; margin-top: 10px">
        <v-col cols="12" sm="6" md="6" lg="6" xl="6" xxl="6">
          <v-card class="cart-data">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-database-outline</v-icon>
              <div class="ms-2 my-auto font-weight-bold">DATA</div>
              <v-btn
                class="d-flex flex-row ms-auto me-2 my-auto"
                style="
                  background: white;
                  color: black;
                  border: 1px solid #cccccc;
                  border-radius: 15px;
                  text-transform: capitalize;
                "
                @click="redirect('/dashboard/dataCollector')"
                >View</v-btn
              >
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
        <v-col cols="12" sm="6" md="6" lg="6" xl="6" xxl="6">
          <v-card class="cart-models-trained">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-robot-outline</v-icon>
              <div class="ms-2 my-auto font-weight-bold">MODELS TRAINED</div>
              <v-btn
                class="d-flex flex-row ms-auto me-2 my-auto"
                style="
                  background: white;
                  color: black;
                  border: 1px solid #cccccc;
                  border-radius: 15px;
                  text-transform: capitalize;
                "
                @click="redirect('/dashboard/modelsTrained')"
                >View</v-btn
              >
            </div>
            <v-row>
              <p
                style="
                  font-size: 1.3rem;
                  font-weight: 400;
                  width: 100%;
                  text-align: center;
                  margin-top: 17px;
                "
              >
                Number of Models
              </p>
            </v-row>
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
                  Mega Models
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{ trainedModelsTable.megaModelCount }}
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
                  Forked Models
                </p>
                <p
                  class="text-center"
                  style="
                    font-size: 1.2rem;
                    line-height: 0.75rem;
                    font-weight: 400;
                  "
                >
                  {{ trainedModelsTable.forkedModelCount }}
                </p>
              </div>
            </div>

            <div
              class="mt-8 mb-12"
              style="height: 1px; background: rgba(211, 211, 211, 0.8)"
            ></div>
            <v-row>
              <apexchart
                :options="trainedModelsTable.dataChartOptions"
                :series="trainedModelsTable.dataSeries"
                type="bar"
                style="width: 100% !important"
                height="300"
              ></apexchart>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-row>
</template>

<script>
import api from "@/components/API";
//==========================

//==========================

export default {
  name: "Dashboard",
  components: {
    [process.browser && "apexchart"]: () => import("vue-apexcharts"),
  },
  head() {
    return {
      title: "Dashboard",
    };
  },
  data() {
    return {
      chatModal: {
        isOpen: false,
        messages: [
          {
            time: new Date(),
            from: "robot",
            message:
              "Hello im your AI assistant, please describe me what kind of information do you want about this Page",
            hasBTN: false,
          },
        ],
        userCommand: "",
        situation: 0,
        isCommandInputDisabled: false,
      },

      allTrackingSelectedCompanies: [],
      summaryCartModels: {
        modelsTrainedCount: 0,
        minProcess: 1000,
        maxProcess: -1,
      },
      allUserAiRobots: [],
      allRecommendedCompanies: [],
      dataPageTable: {
        dataSeries: [],
        dataChartOptions: {},
      },

      summaryRecommendedCompanies: {
        count: 0,
        median: 0,
      },
      allInsightCompanies: [],
      summaryInsights: {
        count: 0,
        min: 1990,
        max: -10,
      },
      trainedModelsTable: {
        dataSeries: [],
        dataChartOptions: {},
        megaModelCount: 0,
        forkedModelCount: 0,
        iqMegaModel: 0,
        iqForkedModel: 0,
      },
      insightTable: {
        dataSeries: [],
        dataChartOptions: {},
        p0: { dataSeries: [], dataChartOptions: {} },
        p1: { dataSeries: [], dataChartOptions: {} },
        p2: { dataSeries: [], dataChartOptions: {} },
      },
    };
  },

  methods: {
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
    redirect(address) {
      window.location.href = address;
    },
    init() {
      this.allInsightCompanies = api.getAllInsightCompanies();
      this.summaryInsights.count = this.allInsightCompanies.length;
      console.log(this.allInsightCompanies);
      this.allInsightCompanies.map((company) => {
        this.summaryInsights.min =
          company.completeness < this.summaryInsights.min
            ? company.completeness
            : this.summaryInsights.min;
        this.summaryInsights.max =
          company.completeness > this.summaryInsights.max
            ? company.completeness
            : this.summaryInsights.max;
      });
      this.allTrackingSelectedCompanies = api.getAllTrackingKPIs();
      this.allUserAiRobots = api.getAllAiRobots();
      this.summaryCartModels.modelsTrainedCount = this.allUserAiRobots.length;
      this.allUserAiRobots.map((robot) => {
        this.summaryCartModels.minProcess =
          parseFloat(robot.totalCompleteness) <
          this.summaryCartModels.minProcess
            ? parseFloat(robot.totalCompleteness)
            : this.summaryCartModels.minProcess;
        this.summaryCartModels.maxProcess =
          parseFloat(robot.totalCompleteness) >
          this.summaryCartModels.maxProcess
            ? parseFloat(robot.totalCompleteness)
            : this.summaryCartModels.maxProcess;
      });

      // =====================
      this.allRecommendedCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        api.getAiRobots()
      );
      const recommendedCompaniesTemp = this.allRecommendedCompanies.filter(
        (x) => x.recommendation.recommended == true
      );
      this.summaryRecommendedCompanies.count = recommendedCompaniesTemp.length;
      let counter = 0;
      recommendedCompaniesTemp.map((x) => {
        this.summaryRecommendedCompanies.median =
          this.summaryRecommendedCompanies.median + parseInt(x.score);
        counter++;
      });
      this.summaryRecommendedCompanies.median = parseFloat(
        this.summaryRecommendedCompanies.median / counter
      ).toFixed(2);
      console.log(this.summaryRecommendedCompanies.median);

      this.fillTrainedModelsTableItems();
      this.fillInsightTableItems();
      this.fillDataPageTable();
    },
    fillTrainedModelsTableItems() {
      const colors = this.getColors();
      const iqLevel = {
        forkedModel: 0,
        megaModel: 0,
        name: "IQ Level",
      };
      const iqBenchMarkLevel = {
        forkedModel: 0,
        megaModel: 0,
        name: "IQ Benchmark",
      };
      const iqImprovementLevel = {
        forkedModel: 0,
        megaModel: 0,
        name: "Last month Improvement",
      };
      let megaCount = 0;
      let forkedCount = 0;
      this.trainedModelsTable.forkedModelCount = this.summaryInsights.count;
      this.trainedModelsTable.megaModelCount =
        this.summaryCartModels.modelsTrainedCount;
      for (let index = 0; index < this.allUserAiRobots.length; index++) {
        const robot = this.allUserAiRobots[index];

        for (
          let iqMegaIndex = 0;
          iqMegaIndex < robot.selectedMegaModels.length;
          iqMegaIndex++
        ) {
          megaCount++;
          iqLevel.megaModel += robot.selectedMegaModels[iqMegaIndex].iqLevel;
          iqBenchMarkLevel.megaModel +=
            robot.selectedMegaModels[iqMegaIndex].benchMark;
          iqImprovementLevel.megaModel +=
            robot.selectedMegaModels[iqMegaIndex].lastMonthIqLevel;
        }
        for (
          let iqMegaIndex = 0;
          iqMegaIndex < robot.selectedForkedModels.length;
          iqMegaIndex++
        ) {
          forkedCount++;
          iqLevel.forkedModel +=
            robot.selectedForkedModels[iqMegaIndex].iqLevel;
          iqBenchMarkLevel.forkedModel +=
            robot.selectedForkedModels[iqMegaIndex].benchMark;
          iqImprovementLevel.forkedModel +=
            robot.selectedForkedModels[iqMegaIndex].lastMonthIqLevel;
        }
      }
      iqLevel.megaModel = parseFloat(iqLevel.megaModel / megaCount).toFixed(2);
      iqLevel.forkedModel = parseFloat(
        iqLevel.forkedModel / forkedCount
      ).toFixed(2);

      iqBenchMarkLevel.megaModel = parseFloat(
        iqBenchMarkLevel.megaModel / megaCount
      ).toFixed(2);
      iqBenchMarkLevel.forkedModel = parseFloat(
        iqBenchMarkLevel.forkedModel / forkedCount
      ).toFixed(2);

      iqImprovementLevel.megaModel = parseFloat(
        iqImprovementLevel.megaModel / megaCount
      );
      iqImprovementLevel.forkedModel = parseFloat(
        iqImprovementLevel.forkedModel / forkedCount
      );

      const modelsTrainedData = [
        {
          forkedModel: this.trainedModelsTable.forkedModelCount,
          megaModel: this.trainedModelsTable.megaModelCount,
          name: "Number of Models",
        },
        iqLevel,
        iqBenchMarkLevel,
        iqImprovementLevel,
      ];
      this.trainedModelsTable.iqForkedModel = parseFloat(
        modelsTrainedData?.[3].megaModel
      ).toFixed(2);
      this.trainedModelsTable.iqMegaModel = parseFloat(
        modelsTrainedData?.[3].forkedModel
      ).toFixed(2);
      const allOptions = this.getEmptyChartObject();
      allOptions.colors = [colors.success, colors.warning];
      allOptions.xaxis.categories = [[`Mega Models`], [`Forked Models`]];
      this.trainedModelsTable.dataChartOptions = { ...allOptions };
      this.trainedModelsTable.dataSeries = [
        {
          name: "IQ Level(Last Month)",
          group: "iqLevel",
          data: [
            (modelsTrainedData?.[1].megaModel * 0.6).toFixed(2),
            (modelsTrainedData?.[1].forkedModel * 0.8).toFixed(2),
          ],
        },
        {
          name: "QI Improvement",
          group: "iqLevel",
          data: [
            (modelsTrainedData?.[1].megaModel * 0.4).toFixed(2),
            (modelsTrainedData?.[1].forkedModel * 0.2).toFixed(2),
          ],
        },
        {
          name: modelsTrainedData?.[2].name,
          group: "benchMark",
          data: [iqBenchMarkLevel.megaModel, iqBenchMarkLevel.forkedModel],
        },
      ];
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
    fillInsightTableItems() {
      const assetPhases = api.getAllAssetPhases();
      const totalAssets = {
        name: "Assets Under Review",
        p0: this.allInsightCompanies.filter((x) => x.assetPhase == 0).length,
        p1: this.allInsightCompanies.filter((x) => x.assetPhase == 1).length,
        p2: this.allInsightCompanies.filter((x) => x.assetPhase == 2).length,
      };
      const avgCompleteness = {
        name: "Assets Insight Completeness",
        p0: 0,
        p1: 0,
        p2: 0,
      };
      const avgPast = {
        name: "Last Month Completed Assets",
        p0: assetPhases[0],
        p1: assetPhases[1],
        p2: assetPhases[2],
      };
      for (let index = 0; index < this.allInsightCompanies.length; index++) {
        const company = this.allInsightCompanies[index];
        if (company.assetPhase == 0)
          avgCompleteness.p0 += parseFloat(company.totalProcessingPercentage);
        if (company.assetPhase == 1)
          avgCompleteness.p1 += parseFloat(company.totalProcessingPercentage);
        if (company.assetPhase == 2)
          avgCompleteness.p2 += parseFloat(company.totalProcessingPercentage);
      }
      if (avgCompleteness.p0 != 0)
        avgCompleteness.p0 = `${parseFloat(
          avgCompleteness.p0 / totalAssets.p0
        ).toFixed(2)}%`;
      else avgCompleteness.p0 = "0%";
      if (avgCompleteness.p1 != 0)
        avgCompleteness.p1 = `${parseFloat(
          avgCompleteness.p1 / totalAssets.p1
        ).toFixed(2)}%`;
      else avgCompleteness.p1 = "0%";
      if (avgCompleteness.p2 != 0)
        avgCompleteness.p2 = `${parseFloat(
          avgCompleteness.p2 / totalAssets.p2
        ).toFixed(2)}%`;
      else avgCompleteness.p2 = "0%";
      const companyInsightsData = [totalAssets, avgCompleteness, avgPast];
      const options = this.getEmptyChartObject();
      options.colors = ["#7c3aed", "#d97706", "#14b8a6"];
      options.plotOptions.bar = {
        horizontal: true,
      };
      options.chart.stacked = false;
      options.chart.toolbar.show = false;
      options.xaxis.categories = [["Phase Zero"], ["Phase One"], ["Phase Two"]];
      this.insightTable.p0.dataSeries = [
        parseFloat(companyInsightsData?.[1].p0).toFixed(2),
      ];
      this.insightTable.p0.dataChartOptions = {
        chart: {
          height: 280,
          type: "radialBar",
        },
        colors: ["#20E647"],
        plotOptions: {
          radialBar: {
            hollow: {
              margin: 0,
              size: "70%",
              background: "#293450",
            },
            track: {
              dropShadow: {
                enabled: true,
                top: 2,
                left: 0,
                blur: 4,
                opacity: 0.15,
              },
            },
            dataLabels: {
              name: {
                offsetY: -10,
                color: "#fff",
                fontSize: "13px",
              },
              value: {
                color: "#fff",
                fontSize: "30px",
                show: true,
              },
            },
          },
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            type: "vertical",
            gradientToColors: ["#609bb5"],
            stops: [0, 100],
          },
        },
        stroke: {
          lineCap: "round",
        },
        labels: ["Phase Zero"],
      };
      this.insightTable.p1.dataSeries = [
        parseFloat(companyInsightsData?.[1].p1).toFixed(2),
      ];
      this.insightTable.p1.dataChartOptions = {
        chart: {
          height: 280,
          type: "radialBar",
        },
        colors: ["#20E647"],
        plotOptions: {
          radialBar: {
            hollow: {
              margin: 0,
              size: "70%",
              background: "#293450",
            },
            track: {
              dropShadow: {
                enabled: true,
                top: 2,
                left: 0,
                blur: 4,
                opacity: 0.15,
              },
            },
            dataLabels: {
              name: {
                offsetY: -10,
                color: "#fff",
                fontSize: "13px",
              },
              value: {
                color: "#fff",
                fontSize: "30px",
                show: true,
              },
            },
          },
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            type: "vertical",
            gradientToColors: ["#609bb5"],
            stops: [0, 100],
          },
        },
        stroke: {
          lineCap: "round",
        },
        labels: ["Phase One"],
      };
      this.insightTable.p2.dataSeries = [
        parseFloat(companyInsightsData?.[1].p2).toFixed(2),
      ];
      this.insightTable.p2.dataChartOptions = {
        chart: {
          height: 280,
          type: "radialBar",
        },
        colors: ["#20E647"],
        plotOptions: {
          radialBar: {
            hollow: {
              margin: 0,
              size: "70%",
              background: "#293450",
            },
            track: {
              dropShadow: {
                enabled: true,
                top: 2,
                left: 0,
                blur: 4,
                opacity: 0.15,
              },
            },
            dataLabels: {
              name: {
                offsetY: -10,
                color: "#fff",
                fontSize: "13px",
              },
              value: {
                color: "#fff",
                fontSize: "30px",
                show: true,
              },
            },
          },
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            type: "vertical",
            gradientToColors: ["#609bb5"],
            stops: [0, 100],
          },
        },
        stroke: {
          lineCap: "round",
        },
        labels: ["Phase Two"],
      };

      this.insightTable.dataChartOptions = { ...options };
      this.insightTable.dataSeries = [
        {
          name: companyInsightsData?.[0].name,
          data: [
            companyInsightsData?.[0].p0,
            companyInsightsData?.[0].p1,
            companyInsightsData?.[0].p2,
          ],
        },
        {
          name: companyInsightsData?.[2].name,
          data: [
            companyInsightsData?.[2].p0,
            companyInsightsData?.[2].p1 == 0
              ? companyInsightsData?.[0].p1
              : companyInsightsData?.[2].p1,
            companyInsightsData?.[2].p2 == 0
              ? companyInsightsData?.[0].p2
              : companyInsightsData?.[2].p2,
          ],
        },
      ];
    },
    chatWithAIBtnSubmitted() {
      if (!this.chatModal.userCommand) {
        this.fireSnack("please write your command first.");
        return;
      }
      this.chatModal.messages.push({
        time: new Date(),
        from: "You",
        message: this.chatModal.userCommand,
        hasBTN: false,
      });
      this.chatModal.messages.push({
        time: new Date(),
        from: "robot",
        message:
          "Mauris luctus eleifend libero at finibus. Morbi et justo varius, convallis risus ut, rutrum nulla. Morbi suscipit facilisis egestas. Nunc sollicitudin accumsan massa et rutrum. Sed bibendum elit vel vulputate lacinia. Phasellus vel fringilla urna. Morbi rhoncus quis mauris quis sodales.",
        hasBTN: false,
      });

      this.chatModal.userCommand = "";
    },
  },
  mounted() {
    if (!api.getAuth()) window.location.href = "/login";

    this.init();
  },
  watch: {},
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

.cart-insights {
  margin-top: 15px;
  padding: 30px !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(166, 252, 255) 100%
  );
  color: black !important;
  padding-left: 0px;
  padding-right: 0px;
}
.cart-deals {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgba(136, 239, 173, 1) 100%
  );
}
.cart-track {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(218, 178, 255) 100%
  );
}
.cart-models-trained {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(191, 191, 191) 100%
  );
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
