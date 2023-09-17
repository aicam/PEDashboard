<template>
  <div>
    <v-row
      class="ps-6 pt-5 pb-6"
      style="font-weight: 400; font-size: 28px; color: black; margin-left: 15px"
    >
      Recommendations
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

      <v-row style="justify-content: space-between">
        <v-col cols="12" sm="12" md="5" lg="5" xl="4" xxl="4">
          <v-card class="cart-deals">
            <div
              class="d-flex flex-row mx-auto"
              style="justify-content: space-between; align-items: center"
            >
              <v-icon large>mdi-shield-check</v-icon>
              <div
                class="my-auto ms-2 font-weight-bold"
                style="text-align: left; width: 100%"
              >
                RECOMMENDATIONS
              </div>
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
        <v-col class="mt-7" style="margin-top: 0px !important; height: 390px">
          <!-- ===============---------------------------===================           -->
          <v-tabs v-model="searchTab" dark grow center>
            <v-tab
              v-for="item in [
                { tab: 'Search With AI' },
                { tab: 'Search Manually' },
              ]"
              :key="item.tab"
            >
              {{ item.tab }}
            </v-tab>
          </v-tabs>

          <v-tabs-items v-model="searchTab">
            <v-tab-item
              v-for="item in [
                { tab: 'Search With AI', id: 0 },
                { tab: 'Search Manually', id: 1 },
              ]"
              :key="item.id"
            >
              <v-container v-if="item.id == 0" style="margin-top: 15px">
                <v-card style="height: 250px; overflow: scroll">
                  <v-card-text>
                    <v-container>
                      <v-row justify="space-around d-flex flex-column">
                        <v-card
                          v-for="message in chatSearchModal.messages"
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
                                <v-img
                                  src="https://via.placeholder.com/50"
                                ></v-img>
                              </v-avatar>
                            </v-list-item-avatar>
                            <v-list-item-content class="received-message">
                              <v-card color="green darken-1" class="flex-none">
                                <v-card-text
                                  class="white--text pa-2 d-flex flex-column"
                                >
                                  <span class="text-caption"
                                    >{{ message.from }}
                                  </span>
                                  <span
                                    class="align-self-start text-subtitle-1"
                                    >{{ message.message }}</span
                                  >
                                  <span
                                    class="text-caption font-italic align-self-end"
                                    >{{ message.time }}</span
                                  >

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
                                        chatSearchModal.situation = -1;
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
                                        chatSearchModal.situation = 1;
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
                            <v-list-item-content
                              class="sent-message justify-end"
                            >
                              <v-card color="primary" class="flex-none">
                                <v-card-text
                                  class="white--text pa-2 d-flex flex-column"
                                >
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
                              <v-img
                                src="https://via.placeholder.com/50"
                              ></v-img>
                            </v-list-item-avatar>
                          </v-list-item>
                        </v-card>
                      </v-row>
                      <v-row>
                        <v-text-field
                          v-model="chatSearchModal.userCommand"
                          :messages="[
                            'Please write your command for search and AI machine will have a conversation with you.',
                          ]"
                        >
                          <v-btn
                            slot="append"
                            text
                            @click="searchChatWithAIBtnSubmitted"
                            :disabled="chatSearchModal.isCommandInputDisabled"
                          >
                            <v-icon color="primary"> mdi-send </v-icon>
                          </v-btn>
                        </v-text-field>
                      </v-row>
                    </v-container>
                  </v-card-text>
                </v-card>
              </v-container>
              <v-container v-if="item.id == 1">
                <v-row
                  class="px-4 mb-8"
                  style="font-weight: 500; font-size: 14px"
                >
                  <v-col cols="12" sm="12" md="12" lg="12" xl="12" xxl="12"
                    >Precision vs Recall</v-col
                  >
                </v-row>
                <v-row style="" class="px-2">
                  <v-col cols="12" sm="12" md="12" lg="12" xl="12" xxl="12">
                    <v-slider
                      v-model="precisionVsRecall"
                      :thumb-size="28"
                      color="black"
                      thumb-label="always"
                      @change="filterByKPI"
                    ></v-slider> </v-col
                ></v-row>
                <v-row>
                  <v-col cols="12" sm="12" md="6" lg="6" xl="6" xxl="6">
                    <v-combobox
                      v-model="allSelectedKpiItems"
                      :items="
                        allSavedKPIs.map((x) => {
                          return {
                            text: `${x.name}: ${x.description}`,
                            value: x.name,
                          };
                        })
                      "
                      @change="filterByKPI"
                      chips
                      clearable
                      label="Select your Standard KPIs"
                      multiple
                      prepend-icon="mdi-filter-variant"
                      solo
                    >
                      <template
                        v-slot:selection="{ attrs, item, select, selected }"
                      >
                        <v-chip
                          v-bind="attrs"
                          :input-value="selected"
                          close
                          @click="select"
                          @click:close="removeKPI(item, 'STANDARD')"
                        >
                          <strong>{{ item.text }}</strong>
                        </v-chip>
                      </template>
                    </v-combobox>
                  </v-col>
                  <v-col cols="12" sm="12" md="6" lg="6" xl="6" xxl="6">
                    <v-combobox
                      v-model="allSelectedAlternativeKpiItems"
                      :items="
                        allSavedAlternativesKPIs.map((x) => {
                          return {
                            text: `${x.name}: ${x.description}`,
                            value: x.name,
                          };
                        })
                      "
                      @change="filterByKPI"
                      chips
                      clearable
                      label="Select your Alternatives KPIs"
                      multiple
                      prepend-icon="mdi-filter-variant"
                      solo
                    >
                      <template
                        v-slot:selection="{ attrs, item, select, selected }"
                      >
                        <v-chip
                          v-bind="attrs"
                          :input-value="selected"
                          close
                          @click="select"
                          @click:close="removeKPI(item, 'ALTERNATIVE')"
                        >
                          <strong>{{ item.text }}</strong>
                        </v-chip>
                      </template>
                    </v-combobox>
                  </v-col>
                </v-row>
              </v-container>
            </v-tab-item>
          </v-tabs-items>
          <!-- ===============---------------------------===================           -->
        </v-col>
      </v-row>

      <v-row>
        <v-container>
          <v-row>
            <v-col
              cols="12"
              xs="12"
              sm="12"
              md="6"
              lg="4"
              xl="3"
              xxl="3"
              v-for="company in allRecommendedCompanies.filter(
                (x) => x.recommendation.recommended == true
              )"
              v-bind:key="company.companyId"
              :style="{
                display:
                  allInsightCompanies.filter(
                    (x) => x.companyId == company.companyId
                  ).length == 0 &&
                  allTrackingCompanies.filter(
                    (x) => x.companyId == company.companyId
                  ).length == 0
                    ? 'none!important'
                    : 'block',
              }"
            >
              <v-card class="mx-auto" style="padding: 20px">
                <v-card-text>
                  <v-row class="d-flex" style="justify-content: space-between">
                    <v-row
                      class="d-flex mt-0 mb-auto"
                      style="justify-content: flex-end"
                    >
                      <v-menu
                        v-model="company.showMenu"
                        absolute
                        offset-y
                        style="max-width: 600px"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <div
                            v-bind="attrs"
                            v-on="on"
                            class="d-flex flex-row"
                            style="
                              width: 30px;
                              height: 30px;
                              border-radius: 50%;
                              font-size: 20px;
                              font-weight: 700;
                              padding-bottom: 10px;
                              border: 1px solid grey;
                              justify-content: center;
                              align-items: center;
                            "
                          >
                            ...
                          </div>
                        </template>
                        <v-list>
                          <v-list-item-group>
                            <v-subheader>Actions</v-subheader>

                            <v-list-item @click="clickOnRobotChat">
                              <v-list-item-icon>
                                <v-icon>mdi-robot-outline</v-icon>
                              </v-list-item-icon>
                              <v-list-item-content>
                                <v-list-item-title
                                  >Need to Talk?</v-list-item-title
                                >
                                <v-list-item-subtitle
                                  >You can talk with AI about this
                                  company</v-list-item-subtitle
                                >
                              </v-list-item-content>
                            </v-list-item>

                            <!-- ================== -->
                            <v-list-item
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
                              <v-list-item-icon>
                                <v-icon>mdi-cloud-print</v-icon>
                              </v-list-item-icon>
                              <v-list-item-content>
                                <v-list-item-title
                                  >Download Summary</v-list-item-title
                                >
                                <v-list-item-subtitle
                                  >You can get summary report about this
                                  company</v-list-item-subtitle
                                >
                              </v-list-item-content>
                            </v-list-item>
                            <!-- ================== -->
                            <v-list-item
                              @click="
                                () => {
                                  fireSnack(
                                    'your review submitted successfully!'
                                  );
                                }
                              "
                            >
                              <v-list-item-icon>
                                <v-icon>mdi-thumb-up</v-icon>
                              </v-list-item-icon>
                              <v-list-item-content>
                                <v-list-item-title>Like</v-list-item-title>
                                <v-list-item-subtitle
                                  >If you are satisfied out of the AI generated
                                  information about this
                                  company</v-list-item-subtitle
                                >
                              </v-list-item-content>
                            </v-list-item>
                            <!-- ================== -->
                            <v-list-item
                              @click="
                                () => {
                                  fireSnack(
                                    'your review submitted successfully!'
                                  );
                                }
                              "
                            >
                              <v-list-item-icon>
                                <v-icon>mdi-thumb-down</v-icon>
                              </v-list-item-icon>
                              <v-list-item-content>
                                <v-list-item-title>Dislike</v-list-item-title>
                                <v-list-item-subtitle
                                  >If you are not satisfied out of the AI
                                  generated information about this
                                  company</v-list-item-subtitle
                                >
                              </v-list-item-content>
                            </v-list-item>
                            <!-- ================== -->
                          </v-list-item-group>
                        </v-list>
                      </v-menu>
                    </v-row>
                  </v-row>
                  <v-row>
                    <div class="text--primary">
                      <div
                        class="d-flex flex-row"
                        style="
                          justify-content: space-between;
                          align-items: center;
                        "
                      >
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
                        <div
                          class="d-flex flex-row me-7 ms-auto my-3"
                          style="justify-content: flex-end"
                        >
                          <v-icon color="success">mdi-currency-usd</v-icon>
                          <div
                            class="d-flex flex-column ms-1"
                            style="align-items: center"
                          >
                            <div style="font-size: 12px; font-weight: 400">
                              Revenue Size: {{ company.revenueSize }}
                            </div>
                          </div>
                        </div>
                      </div>
                      <v-chip-group active-class="primary--text" column>
                        <v-chip class="ma-2" color="#666666" outlined label>
                          Score: {{ company.score }}
                        </v-chip>

                        <v-chip class="ma-2" color="#666666" outlined label>
                          HQ Location: {{ company.hqLocation }}
                        </v-chip>
                      </v-chip-group>
                    </div>
                  </v-row>
                </v-card-text>
                <v-card-actions class="pt-10">
                  <v-btn
                    @click="addCompanyToTracking(company)"
                    class="mt-4"
                    elevation="8"
                    outlined
                    v-if="
                      allTrackingCompanies.filter(
                        (x) => x.companyId == company.companyId
                      ).length == 0
                    "
                  >
                    <v-icon>mdi-plus</v-icon>
                    Tracking list
                  </v-btn>

                  <v-btn
                    v-if="
                      allInsightCompanies.filter(
                        (x) => x.companyId == company.companyId
                      ).length == 0
                    "
                    @click="setSelectForInsight(company)"
                    elevation="8"
                    outlined
                    class="mt-4 mx-3"
                  >
                    <v-icon>mdi-plus</v-icon>
                    Insight list
                  </v-btn>
                </v-card-actions>
              </v-card>
              <!-- =================================== -->
            </v-col>
            <v-col cols="12" v-if="allRecommendedCompanies.length == 0">
              <h2 style="text-align: center">
                Nothing to show! Please change your search filters
              </h2>
            </v-col>
          </v-row>
        </v-container>
      </v-row>

      <!-- Add a review Dialog START -->
      <v-dialog v-model="dialogAddReview.isOpen" max-width="500px">
        <v-card>
          <v-card-title>
            <span class="px-3" style="font-size: 18px; font-weight: 500"
              >Please leave your review about the trained data</span
            >
          </v-card-title>

          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="12" md="12">
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
              @click="dialogAddReview.isOpen = false"
            >
              Cancel
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="
                () => {
                  dialogAddReview.isOpen = false;
                  dialogAddReview.review = '';
                  fireSnack('your review submitted successfully!');
                }
              "
            >
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- Add a review Dialog END -->

      <!-- Focus search Dialog START -->
      <v-dialog v-model="dialogAddFocusSearch.isOpen">
        <v-card>
          <v-card-title>
            <span class="px-3" style="font-size: 18px; font-weight: 500"
              >Please filter the search based on your KPIs. By clicking on '+'
              you can add an AI based custom KPI</span
            >
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
                  <v-text-field outlined label="Input value"> </v-text-field>
                </v-row>
                <v-row class="mt-5">
                  <p class="mt-1 me-3">Label name</p>
                  <v-slider
                    :thumb-size="28"
                    color="black"
                    thumb-label="always"
                  ></v-slider>
                </v-row>

                <v-row
                  class="mt-5 px-3 py-3"
                  style="background: lightskyblue; border-radius: 7px"
                >
                  <p class="mt-3 me-3">Custom KPI</p>
                  <v-text-field outlined label="Input value"> </v-text-field>
                  <v-icon
                    @click="() => console.log('delete custom dialog')"
                    class="mt-0 mb-auto ms-4 pointer"
                    style="
                      font-size: 20px;
                      color: white;
                      background: red;
                      border-radius: 50%;
                    "
                    >mdi-close</v-icon
                  >
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
              @click="dialogAddFocusSearch.isOpen = false"
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
      <v-dialog v-model="dialogSelectFromSearchHistory" max-width="500px">
        <v-card>
          <v-card-title>
            <span class="px-3" style="font-size: 18px; font-weight: 500"
              >Searches history</span
            >
          </v-card-title>

          <v-card-text>
            <v-container>
              <v-col>
                <v-card>
                  <v-col class="p-3">
                    <v-text-field outlined label="Search name"></v-text-field>
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
              @click="dialogAddFocusSearch.isOpen = false"
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
      <v-dialog v-model="dialogAddNewAIBasedKPI" max-width="500px">
        <v-card>
          <v-card-title>
            <span
              class="px-3"
              style="font-size: 18px; font-weight: 500"
              @click="dialogAddNewAIBasedKPI = true"
              >Add new AI based KPI</span
            >
          </v-card-title>

          <v-card-text>
            <v-container>
              <v-col>
                <v-card>
                  <v-col class="p-3">
                    <v-text-field outlined label="KPI label"></v-text-field>
                    <v-text-field outlined label="KPI Type"></v-text-field>

                    <v-spacer></v-spacer>
                    <p>
                      Please write your KPI logic for this input here and AI
                      model will filters the data based on your topic
                    </p>
                    <v-textarea outlined label="KPI logic"></v-textarea>
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
      <v-dialog v-model="dialogSearchFilterName" max-width="500px">
        <v-card>
          <v-card-title>
            <span
              class="px-3"
              style="font-size: 18px; font-weight: 500"
              @click="dialogAddNewAIBasedKPI = true"
              >Please enter name of your search filter</span
            >
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

      <!-- -------------------------    Add NEW KPI Key DIALOG   Start------------------------- -->
      <v-dialog v-model="addNewKpiKeyDialog.isOpen">
        <v-card>
          <v-card-title class="text-h5">
            Adding new
            {{ addNewKpiKeyDialog.type === 0 ? "Standard" : "Alternative" }} KPI
            key filters to the company.
          </v-card-title>
          <v-card-text>
            <v-tabs v-model="tab" dark grow center>
              <v-tab
                v-for="item in [
                  { tab: 'Add KPI manually' },
                  { tab: 'Talk with AI bot' },
                ]"
                :key="item.tab"
              >
                {{ item.tab }}
              </v-tab>
            </v-tabs>

            <v-tabs-items v-model="tab">
              <v-tab-item
                v-for="item in [
                  { tab: 'Add KPI manually', id: 0 },
                  { tab: 'Talk with AI bot', id: 1 },
                ]"
                :key="item.id"
              >
                <v-container v-if="item.id == 0" style="margin-top: 15px">
                  <v-row>
                    <v-text-field
                      style="display: none"
                      label="Enter KPI Name"
                      v-model="addNewKpiKeyDialog.name"
                      hide-details="auto"
                    ></v-text-field>
                  </v-row>
                  <v-row>
                    <v-select
                      v-model="addNewKpiKeyDialog.name"
                      :items="[
                        {
                          text: 'E',
                          isRecommended: true,
                        },
                        {
                          text: 'F',
                          isRecommended: true,
                        },
                        {
                          text: 'G',
                          isRecommended: false,
                        },
                      ]"
                      label="Select KPI"
                    >
                      <template v-slot:item="{ item }">
                        <v-chip v-if="item.isRecommended" color="green">
                          <span>{{ item.text }}</span> (Recommended)
                        </v-chip>
                        <v-chip v-else>
                          <span>{{ item.text }}</span>
                        </v-chip>
                      </template>
                      <template v-slot:selection="{ item, index }">
                        <v-chip v-if="item.isRecommended" color="green">
                          <span>{{ item.text }}</span>
                        </v-chip>
                        <v-chip v-else>
                          <span>{{ item.text }}</span>
                        </v-chip>
                      </template>
                    </v-select>
                  </v-row>
                  <v-row>
                    <v-text-field
                      v-model="addNewKpiKeyDialog.description"
                      label="Enter KPI Logic Commands"
                      hide-details="auto"
                    ></v-text-field>
                  </v-row>
                  <v-row>
                    <v-spacer></v-spacer>
                    <v-btn
                      style="float: right"
                      color="green darken-1"
                      text
                      @click="addNewKpiToUserKpiList(key)"
                    >
                      Add new KPI
                    </v-btn>
                  </v-row>
                </v-container>
                <v-container v-if="item.id == 1">
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
                          <v-card color="primary darken-1" class="flex-none">
                            <v-card-text
                              class="white--text pa-2 d-flex flex-column"
                            >
                              <span class="text-caption"
                                >{{ message.from }}
                              </span>
                              <span class="align-self-start text-subtitle-1">{{
                                message.message
                              }}</span>
                              <span
                                class="text-caption font-italic align-self-end"
                                >{{ message.time }}</span
                              >

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
                            <v-card-text
                              class="white--text pa-2 d-flex flex-column"
                            >
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
                      :messages="[
                        'write your command for your KPI and AI machine will have a conversation with you.',
                      ]"
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
              </v-tab-item>
            </v-tabs-items>

            <v-row style="margin-top: 15px"> </v-row>
            <v-row>KPI Keys:</v-row>
            <v-row
              v-for="key in addNewKpiKeyDialog.type === 0
                ? allSavedKPIs
                : allSavedAlternativesKPIs"
            >
              <p>
                {{ key.name }}:
                {{ key.description ? key.description : "Default" }}
              </p>
              <v-spacer></v-spacer>

              <v-btn
                @click="
                  () => {
                    (addNewKpiKeyDialog.type === 0
                      ? allSavedKPIs
                      : allSavedAlternativesKPIs
                    ).splice(
                      (addNewKpiKeyDialog.type === 0
                        ? allSavedKPIs
                        : allSavedAlternativesKPIs
                      ).findIndex((x) => x.name == key.name),
                      1
                    );
                  }
                "
                >Remove From KPI list</v-btn
              >
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="red darken-1"
              text
              @click="
                () => {
                  addNewKpiKeyDialog.isOpen = false;
                  addNewKpiKeyDialog.description = '';
                  addNewKpiKeyDialog.name = '';
                }
              "
            >
              close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------    Add NEW KPI key DIALOG   END------------------------- -->
    </v-container>
  </div>
</template>

<script>
import utils from "@/components/utils";
import api from "@/components/API";

export default {
  name: "RecommendationPage",
  head() {
    return {
      title: "Recommendations",
    };
  },
  data() {
    return {
      active: [],
      searchTab: null,
      allRecommendedCompanies: [],
      allInsightCompanies: [],
      summaryRecommendedCompanies: {
        count: 0,
        median: 0,
      },
      allAiModels: [],
      snackbar: {
        isOpen: false,
        text: "",
      },
      allTrackingCompanies: [],
      allSavedKPIs: [],
      allSavedAlternativesKPIs: [],
      allSelectedKpiItems: [],
      allSelectedAlternativeKpiItems: [],
      //===============================
      addNewKpiKeyDialog: {
        type: 0,
        isOpen: false,
        name: "",
        description: "",
        companyId: "",
        kpiKeysList: [],

        newSelectedKpiKeys: [],
      },
      chatModal: {
        isOpen: false,
        messages: [
          {
            time: new Date(),
            from: "robot",
            message:
              "Hello im your AI assistant, please describe me what kind of KPI do you want to add.",
            hasBTN: false,
          },
        ],
        userCommand: "",
        situation: 0,
        isCommandInputDisabled: false,
      },
      //===============================
      expanded: [],
      dialogAddReview: {
        isOpen: false,
        review: "",
      },
      dialogAddFocusSearch: {
        isOpen: false,
      },
      dialogSelectFromSearchHistory: false,
      dialogAddNewAIBasedKPI: false,
      dialogSearchFilterName: false,
      headers: [
        {
          text: "Name",
          align: "start",
          sortable: false,
          value: "name",
        },
        { text: "Score", value: "score" },
        { text: "HQ Location", value: "hqLocation" },
        { text: "Revenue Size", value: "revenueSize" },
        { text: "Industry", value: "industry" },
        { text: "Recommended", value: "recommended" },
        { text: "Insight", value: "data-table-expand" },
      ],
      allCompanies: [],
      companies: null,
      precisionVsRecall: 0,
      tab: null,
      currentSearches: [
        {
          label: "0",
          value: "All",
        },
        {
          label: "1",
          value: "Apple",
        },
        {
          label: "2",
          value: "BMW",
        },
        {
          label: "3",
          value: "SpaceX",
        },
        {
          label: "4",
          value: "Tesla",
        },
        {
          label: "5",
          value: "Benz",
        },
        {
          label: "6",
          value: "Coca Cola",
        },
        {
          label: "7",
          value: "Toronto, Ontario, Canada",
        },
        {
          label: "8",
          value: "Los Angeles",
        },
        {
          label: "9",
          value: "Audi",
        },
        {
          label: "10",
          value: "Ottawa",
        },
        {
          label: "11",
          value: "Walmart",
        },
        {
          label: "12",
          value: "New York, USA",
        },
        {
          label: "13",
          value: "BMW",
        },
        {
          label: "14",
          value: "SpaceX",
        },
        {
          label: "15",
          value: "Tesla",
        },
        {
          label: "16",
          value: "Benz",
        },
        {
          label: "17",
          value: "Coca Cola",
        },
        {
          label: "18",
          value: "Toronto, Ontario, Canada",
        },
      ],
      companyTrackSearchInput: "",
      chatSearchModal: {
        isOpen: false,
        messages: [
          {
            time: new Date(),
            from: "robot",
            message:
              "Hello im your AI assistant, please describe what companies are you looking for.",
            hasBTN: false,
          },
        ],
        userCommand: "",
        situation: 0,
        isCommandInputDisabled: false,
      },
    };
  },
  methods: {
    showDataSize(dataSize) {
      return api.dataSizeSerializer(dataSize);
    },
    clickOnRobotChat() {
      document.getElementById("btnStartConversation").click();
    },
    openNewKpiDialog(kpiType) {
      this.addNewKpiKeyDialog.type = kpiType;
      this.addNewKpiKeyDialog.isOpen = true;
    },
    removeKPI(item, type) {
      if (type == "STANDARD")
        this.allSelectedKpiItems.splice(
          this.allSelectedKpiItems.indexOf(item),
          1
        );
      if (type == "ALTERNATIVE")
        this.allSelectedAlternativeKpiItems.splice(
          this.allSelectedAlternativeKpiItems.indexOf(item),
          1
        );
      this.filterByKPI();
    },
    addNewKpiToUserKpiList(kpiKey) {
      if (
        !this.addNewKpiKeyDialog.name ||
        !this.addNewKpiKeyDialog.description
      ) {
        this.fireSnack("all the fields are required!");
        return;
      }
      if (!kpiKey) {
        kpiKey = {
          name: this.addNewKpiKeyDialog.name,
          data: [],
          description: this.addNewKpiKeyDialog.description,
        };
      }

      (this.addNewKpiKeyDialog.type === 0
        ? this.allSavedKPIs
        : this.allSavedAlternativesKPIs
      ).push(kpiKey);
    },

    chatWithAIBtnSubmitted() {
      if (this.chatModal.situation == 0) {
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
            "Here is my information based on the data i learned: `Mauris luctus eleifend libero at finibus. Morbi et justo varius, convallis risus ut, rutrum nulla. Morbi suscipit facilisis egestas. Nunc sollicitudin accumsan massa et rutrum. Sed bibendum elit vel vulputate lacinia. Phasellus vel fringilla urna. Morbi rhoncus quis mauris quis sodales. do you satisfied out of my answer?`",
          hasBTN: true,
        });
        this.chatModal.isCommandInputDisabled = true;
        this.addNewKpiKeyDialog.description = this.chatModal.userCommand;
        this.chatModal.userCommand = "";
        return;
      }
      if (this.chatModal.situation == -1) {
        this.chatModal.messages.map((x) => (x.hasBTN = false));
        this.chatModal.messages.push({
          time: new Date(),
          from: "robot",
          message:
            "sorry my bad!, please give me more information so, i can help you better!",
          hasBTN: false,
        });
        this.chatModal.isCommandInputDisabled = false;
        this.chatModal.userCommand = "";
        this.chatModal.situation = 0;
        return;
      }
      if (this.chatModal.situation == 1) {
        this.chatModal.messages.map((x) => (x.hasBTN = false));
        this.chatModal.messages.push({
          time: new Date(),
          from: "robot",
          message: "Yay, now please give me a name as tag for this KPI",
          hasBTN: false,
        });
        this.chatModal.isCommandInputDisabled = false;
        this.chatModal.userCommand = "";
        this.chatModal.situation = 2;
        return;
      }
      if (this.chatModal.situation == 2) {
        this.chatModal.isCommandInputDisabled = true;
        this.fireSnack("your new KPI added to the KPIs list for this Asset.");
        this.addNewKpiKeyDialog.name = this.chatModal.userCommand;
        this.chatModal.isOpen = false;
        this.chatModal.situation = 0;
        this.chatModal.userCommand = "";
        this.chatModal.messages = [
          {
            time: new Date(),
            from: "robot",
            message:
              "Hello im your AI assistant, please describe me what kind of KPI do you want to add.",
            hasBTN: false,
          },
        ];
        this.addNewKpiToUserKpiList(null);
      }
    },
    filterByKPI() {
      const allAvailableAiRobots = api.getAiRobots();
      this.allRecommendedCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        allAvailableAiRobots
      );
      this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
        (x) => x.score >= this.precisionVsRecall
      );
      if (this.allSelectedKpiItems.length > 0) {
        this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
          (x) =>
            x.userKPIs.some((kpi) =>
              this.allSelectedKpiItems.some((z) => z.value === kpi.name)
            )
        );
      }
      if (this.allSelectedAlternativeKpiItems.length > 0)
        this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
          (x) =>
            x.userKPIs.some((kpi) =>
              this.allSelectedAlternativeKpiItems.some(
                (z) => z.value === kpi.name
              )
            )
        );
    },
    showAIModelByAiId(aiId) {
      return this.allAiModels.filter((x) => x.id == aiId)[0].name;
    },
    addCompanyToTracking(item) {
      this.allTrackingCompanies = api.getAllTrackingKPIs();
      if (!this.allTrackingCompanies.find((x) => x.companyId == item.companyId))
        this.allTrackingCompanies.push(item);
      api.saveTrackingKPIs(this.allTrackingCompanies);
      this.fireSnack("Asset added to tracking list!");
    },
    fireSnack(text) {
      this.snackbar.text = text;
      this.snackbar.isOpen = true;
    },
    filterPerKPI(searchStr = "") {
      const allAvailableAiRobots = api.getAiRobots();
      this.allRecommendedCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        allAvailableAiRobots
      );
      this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
        (x) => x.score >= this.precisionVsRecall
      );
      if (this.allSelectedKpiItems.length > 0)
        this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
          (x) =>
            x.userKPIs.some((kpi) =>
              this.allSelectedKpiItems.includes(kpi.name)
            )
        );
      if (searchStr) {
        this.allRecommendedCompanies = this.allRecommendedCompanies.filter(
          (x) => x.name.toLowerCase().includes(searchStr)
        );
      }
    },
    searchChatWithAIBtnSubmitted() {
      if (!this.chatSearchModal.userCommand) {
        this.fireSnack("please write your command first.");
        return;
      }
      const allAvailableAiRobots = api.getAiRobots();
      this.allRecommendedCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        allAvailableAiRobots
      );
      this.chatSearchModal.messages.push({
        time: new Date(),
        from: "You",
        message: this.chatSearchModal.userCommand,
        hasBTN: false,
      });
      this.chatSearchModal.messages.push({
        time: new Date(),
        from: "robot",
        message:
          "Mauris luctus eleifend libero at finibus. Morbi et justo varius, convallis risus ut, rutrum nulla. Morbi suscipit facilisis egestas. Nunc sollicitudin accumsan massa et rutrum. Sed bibendum elit vel vulputate lacinia. Phasellus vel fringilla urna. Morbi rhoncus quis mauris quis sodales.",
        hasBTN: false,
      });

      this.chatSearchModal.userCommand = "";
      for (let index = 0; index < 4; index++) {
        const randIndex = Math.floor(
          Math.random() * this.allRecommendedCompanies.length
        );
        this.allRecommendedCompanies.splice(randIndex, 1);
      }
      this.fullFillSummary();
    },

    setSelectForInsight(item) {
      this.allInsightCompanies = api.getAllInsightCompanies();
      this.allRecommendedCompanies.map((company) => {
        if (company.companyId == item.companyId) {
          this.allInsightCompanies.push(company);
        }
      });
      console.log(this.allInsightCompanies);

      api.saveInsightCompanies(this.allInsightCompanies);
      this.fireSnack("Companies added successfully!");
    },
    //===============================
    getRandomColor(colorArray) {
      const randomIndex = Math.floor(Math.random() * colorArray.length);
      return colorArray[randomIndex];
    },
    fullFillSummary() {
      this.summaryRecommendedCompanies = {
        count: 0,
        median: 0,
      };
      const recommendedCompaniesTemp = this.allRecommendedCompanies.filter(
        (x) => x.recommendation.recommended == true
      );
      this.summaryRecommendedCompanies.count = recommendedCompaniesTemp.length;
      let counter = 0;
      console.log(this.allRecommendedCompanies, recommendedCompaniesTemp);
      recommendedCompaniesTemp.map((x) => {
        this.summaryRecommendedCompanies.median =
          this.summaryRecommendedCompanies.median + parseInt(x.score);
        counter++;
      });
      this.summaryRecommendedCompanies.median = parseFloat(
        this.summaryRecommendedCompanies.median / counter
      ).toFixed(2);
    },
    init() {
      const allAvailableAiRobots = api.getAiRobots();
      this.allRecommendedCompanies = api.getStandardCompanyList(
        api.getCompanies(),
        allAvailableAiRobots
      );
      this.allInsightCompanies = api.getAllInsightCompanies();
      this.allTrackingCompanies = api.getAllTrackingKPIs();

      this.fullFillSummary();
      this.allAiModels = api.getAiRobots();
      this.allSavedKPIs = api.getAllTrackingKPIKeys();
      this.allSavedAlternativesKPIs = api.getAllTrackingKPIKeys();
      this.active = new Array(this.allSavedKPIs.length + 1);
      for (let index = 0; index < this.active.length; index++) {
        this.active[index] = false;
      }
      this.filterByKPI();

      //============================
      this.allCompanies = utils.getCompanies();
      this.companies = this.allCompanies;
    },
  },
  mounted() {
    if (!api.getAuth()) window.location.href = "/login";
    this.init();
  },
  watch: {
    companyTrackSearchInput(newVal) {
      this.filterPerKPI(newVal);
    },
  },
};
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
</style>
