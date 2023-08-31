<template>
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
    <v-row
      class="ps-3 pt-8 pb-6 mb-3"
      style="font-weight: 600; font-size: 28px; background: black; color: white"
      >Models Trained</v-row
    >
    <v-row style="justify-content: space-between">
      <v-col cols="12" sm="6" md="3" lg="3" xl="3" xxl="3">
        <v-card class="cart-models-trained">
          <v-col class="justify-space-between align-center h-100">
            <p
              class="text-center"
              style="font-size: 2rem; line-height: 2.75rem; font-weight: 700"
            >
              {{ summaryCart.modelsTrainedCount }}
            </p>
            <p
              class="text-center"
              style="font-size: 1.2rem; line-height: 1.5rem; font-weight: 700"
            >
              Models trained
            </p>
            <div style="height: 2px; background: red"></div>
            <div class="candlestick">
              <div class="wick"></div>
              <div
                class="body"
                :style="{
                  width: `${summaryCart.maxProcess - summaryCart.minProcess}%`,
                  left: `${summaryCart.minProcess}%`,
                }"
              ></div>
              <div
                class="shadow-left"
                :style="{ width: `${summaryCart.minProcess}%` }"
              ></div>
              <div
                class="shadow-right"
                :style="{ width: `${summaryCart.maxProcess}%` }"
              ></div>
            </div>
            <v-row class="justify-space-between mt-1 px-2">
              <p style="font-size: 10px">min: 10%</p>
              <p style="font-size: 10px">max: 70%</p>
            </v-row>
          </v-col>
        </v-card>
      </v-col>
      <v-col class="mt-7">
        <v-row style="gap: 10px" class="px-2">
          <v-col cols="12">
            <v-btn
              v-if="allUserAiRobotsIds.length != allAiRobotsIds.length"
              class="ms-3"
              color="black"
              style="color: white; border-radius: 20px; width: 100%"
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
            >
              + Add new Trained AI Robot
            </v-btn>
            <v-btn
              v-else
              class="ms-3"
              color="green dark"
              style="color: white; border-radius: 20px; width: 100%"
            >
              You added all the Trained AI Robots to your account
            </v-btn>
          </v-col>
        </v-row>
        <v-row style="gap: 10px" class="px-2">
          <v-col cols="12">
            <p
              class="ms-3"
              style="
                background: black;
                color: white;
                width: 100%;
                padding: 10px 10px 10px 10px;
                text-align: center;
              "
            >
              Your AI robots data gathering size increased
              <v-chip color="green">10%</v-chip> and data learning increased
              <v-chip color="green">18%</v-chip> than yesterday
            </p>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row>
      <v-card v-for="robot in allUserAiRobots" class="mx-auto" max-width="344">
        <v-card-text>
          <div>{{ robot.nickName }}</div>
          <p class="text-h4 text--primary">{{ robot.name }}</p>
          <div class="text--primary">
            <v-chip-group active-class="primary--text" column>
              <v-chip
                >Total Uploaded Documents: {{ robot.totalUploadedDocsFromUser }}
              </v-chip>
              <v-chip
                >Total data learned from assets:
                {{ showDataSize(robot.totalDataLearnedFromUser) }}
              </v-chip>
              <v-chip
                >Total data gathered for assets:
                {{ showDataSize(robot.totalDataGatheredFromUser) }}
              </v-chip>
              <v-chip
                >Total completeness: {{ robot.totalCompleteness }}%
              </v-chip>
            </v-chip-group>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-btn
            text
            color="green"
            @click="
              () => {
                robotViewDialog.isOpen = true;
                robotViewDialog.data = robot;
              }
            "
          >
            view
          </v-btn>
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
              showDataSize(robotViewDialog.data?.totalDataGatheredFromAllUsers)
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
              showDataSize(robotViewDialog.data?.totalTrainedAssetsFromAllUsers)
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
            color="red darken-1"
            text
            @click="robotViewDialog.isOpen = false"
          >
            close
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            @click="robotViewDialog.isOpen = false"
          >
            Edit Connected Assets
          </v-btn>
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
</template>

<script>
import api from "@/components/API";

export default {
  name: "ModelsTrained",
  head() {
    return {
      title: "Models Trained",
    };
  },
  mounted() {
    if (!api.getAuth()) window.href.location = "/login";
    // const allData = api.getFullUserData();
    // api.saveAiRobots(allData.robots);
    // api.saveCompanies(allData.companies);
    // console.log(allData.robots);
    //======================
    this.init();
  },
  data() {
    return {
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
    };
  },
  methods: {
    fireSnack(text) {
      this.snackbar.text = text;
      this.snackbar.isOpen = true;
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
      api.saveAiRobots(this.allUserAiRobots);
      this.fireSnack("New Model Added Successfully");
      this.init();
    },
    getRobotModelFromId(robotId) {
      return api.getAiRobots().find((x) => x.id == robotId).name;
    },
    showDataSize(dataSize) {
      return api.dataSizeSerializer(dataSize);
    },
    deleteRobotModelAndSave() {
      const index = this.allUserAiRobots.findIndex(
        (x) => x.id == this.robotDeleteDialog.robotId
      );
      this.allUserAiRobots.splice(index, 1);
      api.saveAiRobots(this.allUserAiRobots);
      this.init();
      this.fireSnack("Model deleted successfully");
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
.cart-insights {
  padding-top: 10px !important;
  padding-bottom: 2px !important;
  padding-left: 10px !important;
  padding-right: 10px !important;
  background: linear-gradient(
    82.99deg,
    rgb(92, 105, 226) 5.47%,
    rgb(8, 207, 234) 94.53%
  ) !important;
  color: white !important;
}
.cart-deals {
  padding: 10px !important;
  background: linear-gradient(
    78.84deg,
    rgb(85, 61, 239) 8.24%,
    rgb(207, 95, 228) 91.76%
  ) !important;
  color: white !important;
}
.cart-track {
  padding: 10px !important;
  background: linear-gradient(
    78.84deg,
    rgb(241, 91, 91) 8.24%,
    rgb(177, 36, 121) 91.76%
  ) !important;
  color: white !important;
}
.cart-models-trained {
  padding-top: 10px !important;
  padding-bottom: 2px !important;
  padding-left: 10px !important;
  padding-right: 10px !important;
  background: linear-gradient(
    82.99deg,
    rgb(255, 118, 3) 5.47%,
    rgb(255, 197, 44) 94.53%
  ) !important;
  color: white !important;
}
</style>