<template>
  <div>
    <v-row
      class="ps-6 pt-5 pb-6"
      style="font-weight: 400; font-size: 28px; color: black; margin-left: 15px"
    >
      Models Trained
    </v-row>
    <v-container fluid>
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
      <v-row>
        <v-col cols="12" sm="12" md="12" lg="12" xl="12" xxl="12">
          <v-card class="cart-models-trained">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-robot-outline</v-icon>
              <div
                class="ms-2 my-auto font-weight-bold"
                style="width: 100%; text-align: left"
              >
                MODELS TRAINED
              </div>
              <v-btn
                v-if="
                  allAiRobotsIds
                    .filter((x) => !allUserAiRobotsIds.includes(x))
                    .map((z) => {
                      return { text: getRobotModelFromId(z), value: z };
                    }).length != 0
                "
                class="d-flex flex-row ms-auto me-2 my-auto"
                style="
                  background: white;
                  color: black;
                  border: 1px solid #cccccc;
                  border-radius: 15px;
                  text-transform: capitalize;
                "
                @click="
                  () => {
                    robotAddNewDialog = {
                      isOpen: true,
                      data: {
                        id: '',
                        nickName: '',
                        selectedCompanies: [],
                      },
                    };
                  }
                "
                >+ Add</v-btn
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

      <v-row style="margin-top: 20px">
        <v-tabs v-model="tab" dark grow center>
          <v-tab
            v-for="item in [{ tab: 'Mega Models' }, { tab: 'Forked Models' }]"
            :key="item.tab"
          >
            {{ item.tab }}
          </v-tab>
        </v-tabs>

        <v-tabs-items v-model="tab">
          <v-tab-item
            v-for="item in [
              { tab: 'Mega Models', id: 0 },
              { tab: 'Connected Forked Models Assets', id: 1 },
            ]"
            :key="item.id"
          >
            <v-container v-if="item.id == 0" style="margin-top: 15px">
              <v-row>
                <v-col
                  v-for="robot in allUserAiRobots"
                  cols="12"
                  xs="12"
                  sm="12"
                  md="6"
                  lg="4"
                  xl="4"
                  xxl="4"
                >
                  <v-card class="mx-auto">
                    <v-card-text>
                      <div>{{ robot.nickName }}</div>
                      <p class="text-h4 text--primary">{{ robot.name }}</p>
                      <div class="text--primary">
                        <v-chip-group active-class="primary--text" column>
                          <v-chip
                            >Data Volume:
                            {{ showDataSize(robot.totalDataLearnedFromUser) }}
                          </v-chip>
                          <v-chip
                            >Percentage Trained: {{ robot.totalCompleteness }}%
                          </v-chip>
                          <v-chip>IQ Level: {{ robot.totalIQ }} </v-chip>
                          <v-chip
                            >IQ Benchmark: {{ robot.totalIqBenchmark }}
                          </v-chip>
                          <v-chip
                            >IQ Improvement for last 30 Days:
                            {{ robot.totalIqBenchmarkLast30 }}
                          </v-chip>
                        </v-chip-group>
                      </div>
                    </v-card-text>
                    <v-card-actions>
                      <v-btn
                        text
                        color="red"
                        @click="
                          () => {
                            robotDeleteDialog.isOpen = true;
                            robotDeleteDialog.robotId = robot.id;
                          }
                        "
                      >
                        delete
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
            <v-container v-if="item.id == 1" style="margin-top: 15px">
              <v-row>
                <v-col
                  cols="12"
                  xs="12"
                  sm="12"
                  md="6"
                  lg="6"
                  xl="4"
                  xxl="4"
                  v-for="company in allSavedCompanies"
                  v-bind:key="company.companyId"
                >
                  <v-card class="mx-auto">
                    <v-card-text>
                      <v-row style="margin-left: 5px">
                        <v-row class="ms-2 my-3">
                          <v-avatar
                            color="primary"
                            style="color: white"
                            class="my-auto"
                            size="40"
                          >
                            <img
                              :src="company.companyLogoImg"
                              :alt="company.name"
                            />
                          </v-avatar>
                          <div class="d-flex flex-column ms-2">
                            <div
                              class=""
                              style="font-size: 15px; font-weight: 500"
                            >
                              {{ company.name }}
                            </div>
                            <div
                              class=""
                              style="
                                color: grey;
                                font-size: 13px;
                                font-weight: 400;
                              "
                            >
                              {{ company.industry }}
                            </div>
                          </div>
                          <v-row> </v-row>
                        </v-row>
                      </v-row>
                      <v-row>
                        <v-col
                          cols="6"
                          xs="12"
                          sm="12"
                          md="6"
                          lg="6"
                          xl="6"
                          xxl="6"
                          style="padding: 0px 0px 0px 0px; height: 100%"
                        >
                          <v-timeline align-top dense>
                            <v-timeline-item
                              v-for="message in [
                                {
                                  id: 's1112',
                                  color: 'deep-purple lighten-1',
                                  text: `${parseFloat(
                                    trainedModelsTable.items[0].megaModel /
                                      allSavedCompanies.length
                                  ).toFixed(2)}`,
                                  title: 'IQ Level',
                                },
                                {
                                  id: 's11122',
                                  color: 'deep-purple lighten-1',
                                  text: `${parseFloat(
                                    trainedModelsTable.items[0].megaModel /
                                      allSavedCompanies.length
                                  ).toFixed(2)}`,
                                  title: 'IQ Benchmark',
                                },
                                {
                                  id: 's11123',
                                  color: 'deep-purple lighten-1',
                                  text: `+${parseFloat(
                                    trainedModelsTable.items[0].megaModel /
                                      allSavedCompanies.length
                                  ).toFixed(2)}%`,
                                  title: 'IQ Improvement for last 30 Days',
                                },
                              ]"
                              :key="message.id"
                              :color="message.color"
                              small
                            >
                              <div>
                                <div class="font-weight-normal">
                                  <strong>{{ message.title }}</strong>
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
                          style="padding: 0px 0px 0px 0px; height: 100%"
                        >
                          <v-timeline align-top dense>
                            <v-timeline-item
                              v-for="message in [
                                {
                                  id: 's1112',
                                  color: 'deep-purple lighten-1',
                                  text: showDataSize(company.totalDataGathered),
                                  title: 'Target Data Volume',
                                },
                                {
                                  id: 's11122',
                                  color: 'deep-purple lighten-1',
                                  text: showDataSize(company.totalDataLearned),
                                  title: 'Target Data Trained Volume',
                                },
                                {
                                  id: 's11123',
                                  color: 'deep-purple lighten-1',
                                  text: `${company.totalProcessingPercentage}%`,
                                  title: 'Target Data Training Progress',
                                },
                              ]"
                              :key="message.id"
                              :color="message.color"
                              small
                            >
                              <div>
                                <div class="font-weight-normal">
                                  <strong>{{ message.title }}</strong>
                                </div>
                                <div>{{ message.text }}</div>
                              </div>
                            </v-timeline-item>
                          </v-timeline>
                        </v-col>
                      </v-row>
                    </v-card-text>
                    <v-card-actions>
                      <v-btn
                        text
                        color="green"
                        @click="
                          () => {
                            fireSnack(
                              `${company.name.replace(
                                ' ',
                                ''
                              )}-${+new Date()}.pdf Sent to your email!`
                            );
                          }
                        "
                      >
                        Download Report
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
          </v-tab-item>
        </v-tabs-items>
      </v-row>
      <!-- -------------------------AI ROBOT DIALOG START---------------- -->
      <v-dialog v-model="robotViewDialog.isOpen" max-width="500">
        <v-card>
          <v-card-title class="text-h5">
            {{ robotViewDialog.data?.nickName }}
            <v-chip color="green">{{ robotViewDialog.data?.name }}</v-chip>
          </v-card-title>

          <v-card-text>
            <v-chip
              >Total Data Gathered From All Users:
              {{
                showDataSize(
                  robotViewDialog.data?.totalDataGatheredFromAllUsers
                )
              }}</v-chip
            >
            <v-chip
              >Total Data Learned From All Users:
              {{
                showDataSize(robotViewDialog.data?.totalDataLearnedFromAllUsers)
              }}</v-chip
            >
            <v-chip>Total Used: {{ robotViewDialog.data?.totalUsed }}</v-chip>
            <v-chip
              >Total Trained Assets From All Users:
              {{
                showDataSize(
                  robotViewDialog.data?.totalTrainedAssetsFromAllUsers
                )
              }}</v-chip
            >
            <v-chip
              >Total Uploaded Docs From User:
              {{ robotViewDialog.data?.totalUploadedDocsFromUser }}</v-chip
            >
            <v-chip
              >Total Data Learned From User:
              {{
                showDataSize(robotViewDialog.data?.totalDataLearnedFromUser)
              }}</v-chip
            >
            <v-chip
              >Total Data Gathered From User:
              {{
                showDataSize(robotViewDialog.data?.totalDataGatheredFromUser)
              }}</v-chip
            >
            <v-chip
              >Total Completeness:
              {{ robotViewDialog.data?.totalCompleteness }}%</v-chip
            >
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="red darken-1"
              text
              @click="
                () => {
                  robotDeleteDialog.isOpen = true;
                  robotDeleteDialog.robotId = robotViewDialog.data?.id;
                }
              "
            >
              delete
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn
              color="green darken-1"
              text
              @click="robotViewDialog.isOpen = false"
            >
              close
            </v-btn>
            <!-- <v-btn
            color="green darken-1"
            text
            @click="robotViewDialog.isOpen = false"
          >
            Edit Connected Assets
          </v-btn> -->
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------AI ROBOT DIALOG          END------------------------- -->
      <!-- -------------------------AI ROBOT DELETE DIALOG START------------------------- -->
      <v-dialog v-model="robotDeleteDialog.isOpen" max-width="500">
        <v-card>
          <v-card-title class="text-h5">
            are you sure to delete this Model?
          </v-card-title>

          <v-card-actions>
            <v-btn color="red darken-1" text @click="deleteRobotModelAndSave()">
              Yes
            </v-btn>
            <v-spacer></v-spacer>

            <v-btn
              color="green darken-1"
              text
              @click="
                () => {
                  robotDeleteDialog.isOpen = false;
                  robotDeleteDialog.robotId = null;
                }
              "
            >
              No
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------AI ROBOT DELETE DIALOG   END------------------------- -->
      <!-- -------------------------AI ROBOT DELETE DIALOG START------------------------- -->
      <v-dialog v-model="robotAddNewDialog.isOpen" max-width="500">
        <v-card>
          <v-card-title class="text-h5">
            Adding new AI Robots Model
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-text-field
                label="Your Robot Nick Name"
                v-model="robotAddNewDialog.data.nickName"
              ></v-text-field>
            </v-row>
            <v-row>
              <v-select
                v-model="robotAddNewDialog.data.id"
                :items="
                  allAiRobotsIds
                    .filter((x) => !allUserAiRobotsIds.includes(x))
                    .map((z) => {
                      return { text: getRobotModelFromId(z), value: z };
                    })
                "
                label="Select Your AI Model"
                outlined
              ></v-select>
            </v-row>
            <v-row>
              <v-autocomplete
                solo
                multiple
                v-model="robotAddNewDialog.data.selectedCompanies"
                :items="
                  allCompanies.map((company) => {
                    return {
                      text: company.name,
                      value: company.companyId,
                      raw: company,
                    };
                  })
                "
              ></v-autocomplete>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="red darken-1"
              text
              @click="() => (robotAddNewDialog.isOpen = false)"
            >
              close
            </v-btn>
            <v-spacer></v-spacer>

            <v-btn color="green darken-1" text @click="addNewAiRobot()">
              Add
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------AI ROBOT DELETE DIALOG   END------------------------- -->
    </v-container>
  </div>
</template>

<script>
import api from "@/components/API";

export default {
  name: "ModelsTrained",
  components: {
    [process.browser && "apexchart"]: () => import("vue-apexcharts"),
  },
  head() {
    return {
      title: "Models Trained",
    };
  },
  mounted() {
    if (!api.getAuth()) window.location.href = "/login";
    // const allData = api.getFullUserData();
    // api.saveAiRobots(allData.robots);
    // api.saveCompanies(allData.companies);
    // console.log(allData.robots);
    //======================
    this.init();
  },

  data() {
    return {
      tab: null,

      trainedModelsTable: {
        dataSeries: [],
        dataChartOptions: {},
        megaModelCount: 0,
        forkedModelCount: 0,
        iqMegaModel: 0,
        iqForkedModel: 0,
        items: [],
      },
      summaryCart: {
        modelsTrainedCount: 0,
        minProcess: 1000,
        maxProcess: -1,
      },
      allUserAiRobots: [],
      snackbar: {
        isOpen: false,
        text: "",
      },
      robotAddNewDialog: {
        isOpen: false,
        data: {
          id: "",
          nickName: "",
          selectedCompanies: [],
        },
      },
      robotViewDialog: {
        isOpen: false,
        data: null,
      },
      robotDeleteDialog: {
        isOpen: false,
        robotId: null,
      },
      allAiRobotsIds: [],
      allUserAiRobotsIds: [],
      allCompanies: [],
      allSavedCompanies: [],
    };
  },
  methods: {
    fireSnack(text) {
      this.snackbar.text = text;
      this.snackbar.isOpen = true;
    },
    showDataSize(dataSize) {
      return api.dataSizeSerializer(dataSize);
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
      this.trainedModelsTable.forkedModelCount = this.allSavedCompanies.length;
      this.trainedModelsTable.megaModelCount = this.allUserAiRobots.length;
      for (let index = 0; index < this.allUserAiRobots.length; index++) {
        const robot = this.allUserAiRobots[index];
        robot["totalIQ"] = 0;
        robot["totalIqBenchmark"] = 0;
        robot["totalIqBenchmarkLast30"] = 0;
        const megaModelsCount = robot.selectedMegaModels.length;
        for (
          let iqMegaIndex = 0;
          iqMegaIndex < megaModelsCount;
          iqMegaIndex++
        ) {
          megaCount++;
          robot["totalIQ"] += robot.selectedMegaModels[iqMegaIndex].iqLevel;
          robot["totalIqBenchmark"] +=
            robot.selectedMegaModels[iqMegaIndex].benchMark;
          robot["totalIqBenchmarkLast30"] +=
            robot.selectedMegaModels[iqMegaIndex].lastMonthIqLevel;
          iqLevel.megaModel += robot.selectedMegaModels[iqMegaIndex].iqLevel;
          iqBenchMarkLevel.megaModel +=
            robot.selectedMegaModels[iqMegaIndex].benchMark;
          iqImprovementLevel.megaModel +=
            robot.selectedMegaModels[iqMegaIndex].lastMonthIqLevel;
        }
        robot["totalIQ"] = (robot["totalIQ"] / megaModelsCount).toFixed(2);
        robot["totalIqBenchmark"] = (
          robot["totalIqBenchmark"] / megaModelsCount
        ).toFixed(2);
        robot["totalIqBenchmarkLast30"] = (
          robot["totalIqBenchmarkLast30"] / megaModelsCount
        ).toFixed(2);

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
        this.allUserAiRobots[index] = robot;
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
      this.trainedModelsTable.items = [
        {
          forkedModel: this.trainedModelsTable.forkedModelCount,
          megaModel: this.trainedModelsTable.megaModelCount,
          name: "Number of Models",
        },
        iqLevel,
        iqBenchMarkLevel,
        iqImprovementLevel,
      ];
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
    init() {
      this.allUserAiRobots = api.getAllAiRobots();
      this.allCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        api.getAiRobots()
      );
      this.allUserAiRobots.map((robot) =>
        this.allUserAiRobotsIds.push(robot.id)
      );
      api.getAiRobots().map((robot) => this.allAiRobotsIds.push(robot.id));
      this.robotViewDialog = {
        isOpen: false,
        data: null,
      };
      this.robotDeleteDialog = {
        isOpen: false,
        robotId: null,
      };
      this.robotAddNewDialog = {
        isOpen: false,
        data: {
          id: "",
          nickName: "",
        },
      };
      this.robotAddNewDialog.isOpen = false;
      this.robotAddNewDialog.data.id = "";
      this.robotAddNewDialog.data.nickName = "";
      this.robotAddNewDialog.data["selectedCompanies"] = api
        .getAllCompanies()
        .map((x) => {
          return {
            text: x.name,
            value: x.companyId,
            raw: x,
          };
        });
      this.summaryCart.modelsTrainedCount = this.allUserAiRobots.length;

      this.allUserAiRobots.map((robot) => {
        this.summaryCart.minProcess =
          parseFloat(robot.totalCompleteness) < this.summaryCart.minProcess
            ? parseFloat(robot.totalCompleteness)
            : this.summaryCart.minProcess;
        this.summaryCart.maxProcess =
          parseFloat(robot.totalCompleteness) > this.summaryCart.maxProcess
            ? parseFloat(robot.totalCompleteness)
            : this.summaryCart.maxProcess;
      });
      this.allSavedCompanies = api.getAllCompanies();
      this.trainedModelsTable.forkedModelCount = this.allSavedCompanies.length;
      this.trainedModelsTable.megaModelCount = this.allUserAiRobots.length;
      this.fillTrainedModelsTableItems();
    },
    generateForkedModelsRelation(company) {
      return this.allUserAiRobots[
        this.allUserAiRobotsIds.findIndex((z) => z == company.aiModelsID)
      ].name;
    },
    addNewAiRobot() {
      if (
        !this.robotAddNewDialog.data.id ||
        !this.robotAddNewDialog.data.nickName ||
        this.robotAddNewDialog.data.selectedCompanies.length == 0
      ) {
        this.fireSnack("all the fields are required");
        return;
      }
      const userSavedCompanies = api.getAllCompanies();
      this.robotAddNewDialog.data.selectedCompanies.map((companyId) => {
        if (!userSavedCompanies.includes((x) => x.companyId == companyId)) {
          const index = this.allCompanies.findIndex(
            (z) => z.companyId == companyId
          );
          userSavedCompanies.push(this.allCompanies[index]);
        }
      });
      api.saveCompanies(userSavedCompanies);
      //===========
      const allAvailableRobots = api.getAiRobots();
      let robotIndex = allAvailableRobots.findIndex(
        (x) => x.id == this.robotAddNewDialog.data.id
      );
      allAvailableRobots[robotIndex] = api.generateAiRobotsBasedOnUserData(
        [allAvailableRobots[robotIndex]],
        userSavedCompanies
      );

      allAvailableRobots[robotIndex][0]["nickName"] =
        this.robotAddNewDialog.data.nickName;
      this.allUserAiRobots.push(allAvailableRobots[robotIndex][0]);
      this.fillTrainedModelsTableItems();
      api.saveAiRobots(this.allUserAiRobots);
      this.fireSnack("New Model Added Successfully");
      this.init();
    },
    getRobotModelFromId(robotId) {
      return api.getAiRobots().find((x) => x.id == robotId).name;
    },

    deleteRobotModelAndSave() {
      const index = this.allUserAiRobots.findIndex(
        (x) => x.id == this.robotDeleteDialog.robotId
      );
      this.allUserAiRobots.splice(index, 1);
      api.saveAiRobots(this.allUserAiRobots);
      this.fireSnack("Model deleted successfully, Reloading Data!");
      window.location.reload();
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
.cart-models-trained {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(191, 191, 191) 100%
  );
}
</style>
