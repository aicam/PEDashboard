<template>
  <div>
    <v-row
      class="ps-6 pt-5 pb-6"
      style="font-weight: 400; font-size: 28px; color: black; margin-left: 15px"
    >
      Tracking Assets
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
        <v-col cols="12" sm="12" md="4" lg="4" xl="4" xxl="4">
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
                @click="() => (addNewTrackingCompanyDialog.isOpen = true)"
              >
                + Add</v-btn
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
        <!-- ======================================11111111111================ -->
        <v-col class="mt-7" style="margin-top: 0px !important; height: 390px">
          <!-- ===============---------------------------===================           -->
          <v-tabs v-model="searchTab1" dark grow center>
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

          <v-tabs-items v-model="searchTab1">
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
                </v-row></v-container
              >
            </v-tab-item>
          </v-tabs-items>
          <!-- ===============---------------------------===================           -->
        </v-col>
        <!-- ======================================11111111111================ -->
      </v-row>
      <v-row>
        <v-col
          cols="12"
          xs="12"
          sm="12"
          md="6"
          lg="4"
          xl="3"
          xxl="3"
          v-for="company in allTrackingSelectedCompanies"
          v-bind:key="company.companyId"
        >
          <v-card class="mx-auto" style="padding: 20px">
            <v-card-text>
              <v-row class="d-flex" style="justify-content: space-between">
                <v-row class="d-flex" style="justify-content: flex-start">
                  <v-chip
                    v-if="company.readyToBuy"
                    class="ma-2"
                    color="success"
                    outlined
                    label
                  >
                    Ready to Buy
                  </v-chip>
                </v-row>
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
                            <v-list-item-title>Need to Talk?</v-list-item-title>
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
                              >Download Report</v-list-item-title
                            >
                            <v-list-item-subtitle
                              >You can get tracking report out of this
                              company</v-list-item-subtitle
                            >
                          </v-list-item-content>
                        </v-list-item>
                        <!-- ================== -->
                        <v-list-item
                          @click="
                            () => {
                              fireSnack('your review submitted successfully!');
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
                              fireSnack('your review submitted successfully!');
                            }
                          "
                        >
                          <v-list-item-icon>
                            <v-icon>mdi-thumb-down</v-icon>
                          </v-list-item-icon>
                          <v-list-item-content>
                            <v-list-item-title>Dislike</v-list-item-title>
                            <v-list-item-subtitle
                              >If you are not satisfied out of the AI generated
                              information about this
                              company</v-list-item-subtitle
                            >
                          </v-list-item-content>
                        </v-list-item>
                        <!-- ================== -->
                        <v-list-item
                          @click="
                            () => {
                              deleteTrackingDialog.isOpen = true;
                              deleteTrackingDialog.companyId =
                                company.companyId;
                            }
                          "
                        >
                          <v-list-item-icon>
                            <v-icon>mdi-delete-outline</v-icon>
                          </v-list-item-icon>
                          <v-list-item-content>
                            <v-list-item-title>Remove</v-list-item-title>
                            <v-list-item-subtitle
                              >You can remove this company from your tracking
                              list</v-list-item-subtitle
                            >
                          </v-list-item-content>
                        </v-list-item>
                      </v-list-item-group>
                    </v-list>
                  </v-menu>
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
                      <v-chip
                        :color="`${company.userKPIs.length == 0 ? 'red' : ''}`"
                        class="ma-2"
                        outlined
                        label
                      >
                        Selected Tracking KPIs: {{ company.userKPIs.length }}
                      </v-chip>

                      <v-chip class="ma-2" color="#666666" outlined label>
                        Score: {{ company.score }}
                      </v-chip>

                      <v-chip class="ma-2" color="#666666" outlined label>
                        HQ Location: {{ company.hqLocation }}
                      </v-chip>
                    </v-chip-group>
                  </div>
                  <div class="text--primary" v-if="false">
                    you need to add KPI to this company!
                  </div>
                </v-row>
              </v-row>
            </v-card-text>
            <v-card-actions class="pt-10">
              <v-btn
                block
                elevation="8"
                outlined
                color="green"
                @click="
                  () => {
                    viewTrackingCompanyDialog.isOpen = true;
                    viewTrackingCompanyDialog.company = company;
                    changeKPIChart(0);
                  }
                "
              >
                <v-icon left> mdi-eye-outline </v-icon>
                view
              </v-btn>
            </v-card-actions>
          </v-card>
          <!-- =================================== -->
        </v-col>
      </v-row>
      <!-- -------------------------Tracking View DIALOG START------------------------- -->
      <v-dialog v-model="viewTrackingCompanyDialog.isOpen" max-width="600">
        <v-card>
          <v-card-title> Tracking overview </v-card-title>
          <v-card-text>
            <v-row style="padding: 10px 10px 10px 10px">
              <p class="text-h4 text--primary">
                {{ viewTrackingCompanyDialog.company?.name }}
              </p>
            </v-row>
            <v-row>
              <v-chip-group mandatory active-class="primary--text">
                <v-chip
                  v-for="(tag, i) in viewTrackingCompanyDialog.company
                    ?.userKPIs"
                  :key="tag.name"
                  @click="changeKPIChart(i)"
                >
                  {{ tag.name }}
                </v-chip>
              </v-chip-group>
              <v-spacer></v-spacer>

              <v-btn
                text
                color="green darken-1"
                @click="
                  openNewKpiDialog(viewTrackingCompanyDialog.company.companyId)
                "
                >Manage Standard KPIs</v-btn
              >
            </v-row>
            <v-row>
              <v-chip-group mandatory active-class="primary--text">
                <v-chip
                  v-for="(tag, i) in viewTrackingCompanyDialog.company
                    ?.userAlternativeKPIs"
                  :key="tag.name"
                  @click="changeKPIChart(i, 'userAlternativeKPIs')"
                >
                  {{ tag.name }}
                </v-chip>
              </v-chip-group>
              <v-spacer></v-spacer>

              <v-btn
                text
                color="green darken-1"
                @click="
                  openNewKpiDialog(
                    viewTrackingCompanyDialog.company.companyId,
                    'userAlternativeKPIs'
                  )
                "
                >Manage Alternative KPIs</v-btn
              >
            </v-row>
            <v-row>
              <apexchart
                :options="chartOptions"
                :series="chartSeries"
                type="line"
                style="width: 100% !important"
                height="300"
              ></apexchart>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="red darken-1"
              text
              @click="
                () => {
                  deleteTrackingDialog.isOpen = true;
                  deleteTrackingDialog.companyId =
                    viewTrackingCompanyDialog.company.companyId;
                }
              "
            >
              Delete
            </v-btn>
            <v-spacer></v-spacer>

            <v-btn
              color="green darken-1"
              text
              @click="
                () => {
                  viewTrackingCompanyDialog.isOpen = false;
                  viewTrackingCompanyDialog.company = null;
                }
              "
            >
              close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------Add View DIALOG        END------------------------- -->
      <!-- -------------------------Tracking Add new DIALOG START------------------------- -->
      <v-dialog v-model="addNewTrackingCompanyDialog.isOpen">
        <v-card>
          <v-card-title class="text-h5">
            Select Your Favorite Companies to Track
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-text-field
                  label="Search your target company"
                  :messages="[
                    'this search input uses AI, feel free to talk with it and it will filter the companies based on your search command',
                  ]"
                  v-model="companyTrackSearchInput"
                ></v-text-field>
              </v-row>
              <v-row>
                <v-col
                  cols="12"
                  xs="12"
                  sm="12"
                  md="6"
                  lg="4"
                  xl="3"
                  xxl="3"
                  v-for="company in addNewTrackingCompanyDialog.allCompanies"
                >
                  <v-card
                    v-bind:key="company.companyId"
                    class="mx-auto mt-5"
                    style="padding: 20px"
                  >
                    <v-card-text>
                      <v-row
                        class="d-flex"
                        style="justify-content: space-between"
                      >
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
                                <v-icon color="success"
                                  >mdi-currency-usd</v-icon
                                >
                                <div
                                  class="d-flex flex-column ms-1"
                                  style="align-items: center"
                                >
                                  <div
                                    style="font-size: 12px; font-weight: 400"
                                  >
                                    Revenue Size: {{ company.revenueSize }}
                                  </div>
                                </div>
                              </div>
                            </div>
                            <v-chip-group active-class="primary--text" column>
                              <v-chip
                                class="ma-2"
                                color="#666666"
                                outlined
                                label
                              >
                                Score: {{ company.score }}
                              </v-chip>
                              <v-chip
                                class="ma-2"
                                color="#666666"
                                outlined
                                label
                              >
                                HQ Location: {{ company.hqLocation }}
                              </v-chip>
                            </v-chip-group>
                          </div>
                        </v-row>
                      </v-row>
                    </v-card-text>
                    <v-card-actions class="pt-10">
                      <v-btn
                        block
                        elevation="8"
                        outlined
                        color="green"
                        @click="
                          () => {
                            company.isSelectedAttr = true;
                            viewTrackingCompanyDialog.company =
                              addCompanyToTrackingSystem();
                            viewTrackingCompanyDialog.isOpen = true;
                            changeKPIChart(0);
                          }
                        "
                      >
                        <v-icon left> mdi-plus </v-icon>
                        Add to tracking company list
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-col>

                <!-- ================= -->
              </v-row></v-container
            >
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="red darken-1"
              text
              @click="
                () => {
                  addNewTrackingCompanyDialog.isOpen = false;
                }
              "
            >
              close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------Add New DIALOG   END------------------------- -->
      <!-- -------------------------    DELETE DIALOG   Start------------------------- -->
      <v-dialog v-model="deleteTrackingDialog.isOpen" max-width="600">
        <v-card>
          <v-card-title class="text-h5">
            are you sure to delete this Tracked Company?
          </v-card-title>

          <v-card-actions>
            <v-btn
              color="red darken-1"
              text
              @click="deleteTrackingCompany(deleteTrackingDialog.companyId)"
            >
              Yes
            </v-btn>
            <v-spacer></v-spacer>

            <v-btn
              color="green darken-1"
              text
              @click="
                () => {
                  deleteTrackingDialog.isOpen = false;
                  deleteTrackingDialog.companyId = '';
                }
              "
            >
              No
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------    DELETE DIALOG   END------------------------- -->

      <!-- -------------------------    Add NEW KPI Key DIALOG   Start------------------------- -->
      <v-dialog v-model="addNewKpiKeyDialog.isOpen" max-width="800">
        <v-card>
          <v-card-title class="text-h5">
            Adding new
            {{
              addNewKpiKeyDialog.kpiType == "userKPIs"
                ? "KPI"
                : "Alternative KPI"
            }}
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
                  <v-row style="display: none">
                    <v-text-field
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
                          text: 'A',
                          isRecommended: true,
                        },
                        {
                          text: 'B',
                          isRecommended: true,
                        },
                        {
                          text: 'C',
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
            <v-row v-for="key in addNewKpiKeyDialog.newSelectedKpiKeys">
              <v-col cols="12" sm="12" md="12" lg="8" xl="8" xxl="8">
                <p>
                  {{ key.name }}:
                  {{ key.description ? key.description : "Default" }}
                </p>
              </v-col>
              <v-col cols="12" sm="12" md="12" lg="4" xl="4" xxl="4">
                <v-btn
                  block
                  v-if="hasKpiKey(key)"
                  @click="addNewKpiToUserKpiList(key)"
                  >Add To KPI List</v-btn
                >
                <v-btn block v-else @click="removeKpiToUserKpiList(key)"
                  >Remove From KPI list</v-btn
                >
              </v-col>
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
                  addNewKpiKeyDialog.kpiKeysList = [];
                }
              "
            >
              close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- -------------------------    Add NEW KPI key DIALOG   END------------------------- -->
      <!-- -------------------------    Add NEW KPI Key DIALOG   Start------------------------- -->
      <v-dialog v-model="addNewKpiKeyDialogForSearch.isOpen">
        <v-card>
          <v-card-title class="text-h5">
            Adding new
            {{
              addNewKpiKeyDialogForSearch.type === 0
                ? "Standard"
                : "Alternative"
            }}
            KPI key filters to the company.
          </v-card-title>
          <v-card-text>
            <v-tabs v-model="searchTab" dark grow center>
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

            <v-tabs-items v-model="searchTab">
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
                      v-model="addNewKpiKeyDialogForSearch.name"
                      hide-details="auto"
                    ></v-text-field>
                  </v-row>
                  <v-row>
                    <v-select
                      v-model="addNewKpiKeyDialogForSearch.name"
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
                      v-model="addNewKpiKeyDialogForSearch.description"
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
                      @click="addNewKpiToUserKpiListForSearch(key)"
                    >
                      Add new KPI
                    </v-btn>
                  </v-row>
                </v-container>
                <v-container v-if="item.id == 1">
                  <v-row justify="space-around d-flex flex-column">
                    <v-card
                      v-for="message in chatModalForSearch.messages"
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
                                    chatModalForSearch.situation = -1;
                                    chatWithAIBtnSubmittedForSearch();
                                  }
                                "
                              >
                                No
                              </v-btn>
                              <v-btn
                                color="green"
                                @click="
                                  () => {
                                    chatModalForSearch.situation = 1;
                                    chatWithAIBtnSubmittedForSearch();
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
                      v-model="chatModalForSearch.userCommand"
                      :disabled="chatModalForSearch.isCommandInputDisabled"
                      :messages="[
                        'write your command for your KPI and AI machine will have a conversation with you.',
                      ]"
                    >
                      <v-btn
                        slot="append"
                        text
                        @click="chatWithAIBtnSubmittedForSearch"
                        :disabled="chatModalForSearch.isCommandInputDisabled"
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
              v-for="key in addNewKpiKeyDialogForSearch.type === 0
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
                    (addNewKpiKeyDialogForSearch.type === 0
                      ? allSavedKPIs
                      : allSavedAlternativesKPIs
                    ).splice(
                      (addNewKpiKeyDialogForSearch.type === 0
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
                  addNewKpiKeyDialogForSearch.isOpen = false;
                  addNewKpiKeyDialogForSearch.description = '';
                  addNewKpiKeyDialogForSearch.name = '';
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
import api from "@/components/API";

export default {
  name: "Tracking",
  components: {
    [process.browser && "apexchart"]: () => import("vue-apexcharts"),
  },
  head() {
    return {
      title: "Tracking",
    };
  },
  watch: {
    companyTrackSearchInput(newVal) {
      if (newVal) {
        this.addNewTrackingCompanyDialog.allCompanies =
          this.addNewTrackingCompanyDialog.allCompanies.filter((x) =>
            x.name.toLowerCase().includes(newVal)
          );
      } else {
        this.addNewTrackingCompanyDialog.allCompanies = api
          .getStandardCompanyList(api.getCompanies(), api.getAiRobots())
          .filter(
            (z) =>
              this.allTrackingSelectedCompanies.findIndex(
                (k) => k.companyId == z.companyId
              ) == -1
          );
      }
    },
  },
  mounted() {
    if (!api.getAuth()) window.location.href = "/login";
    // const allData = api.getFullUserData();
    // api.saveAiRobots(allData.robots);
    // api.saveCompanies(allData.companies);
    // api.saveTrackingKPIs(allData.trackingCompaniesKPIs);
    // api.saveTrackingKPIKeys(allData.trackingKpiKeys);
    // console.log(JSON.stringify(allData));
    //======================
    this.init();
  },

  data() {
    return {
      showMenu: false,
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
      tab: null,
      searchTab: null,
      snackbar: {
        isOpen: false,
        text: "",
      },
      deleteTrackingDialog: {
        isOpen: false,
        companyId: "",
      },
      addNewTrackingCompanyDialog: {
        isOpen: false,
        allCompanies: [],
      },
      addNewKpiKeyDialog: {
        isOpen: false,
        name: "",
        description: "",
        companyId: "",
        kpiKeysList: [],
        kpiType: "",
        newSelectedKpiKeys: [],
      },
      viewTrackingCompanyDialog: {
        isOpen: false,
        company: null,
        selectedKpiIndex: 0,
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
      allTrackingSelectedCompanies: [],
      chartSeries: [],
      searchTab1: null,
      chartOptions: {},
      companyTrackSearchInput: "",
      //===============================
      precisionVsRecall: 0,
      allSelectedKpiItems: [],
      allSavedKPIs: [],
      allSelectedAlternativeKpiItems: [],
      allSavedAlternativesKPIs: [],
      addNewKpiKeyDialogForSearch: {
        type: 0,
        isOpen: false,
        name: "",
        description: "",
        companyId: "",
        kpiKeysList: [],

        newSelectedKpiKeys: [],
      },
      chatModalForSearch: {
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
        KKK: null,
      },
      //===============================
    };
  },
  methods: {
    chatWithAIBtnSubmittedForSearch() {
      if (this.chatModalForSearch.situation == 0) {
        if (!this.chatModalForSearch.userCommand) {
          this.fireSnack("please write your command first.");
          return;
        }
        this.chatModalForSearch.messages.push({
          time: new Date(),
          from: "You",
          message: this.chatModalForSearch.userCommand,
          hasBTN: false,
        });
        this.chatModalForSearch.messages.push({
          time: new Date(),
          from: "robot",
          message:
            "Here is my information based on the data i learned: `Mauris luctus eleifend libero at finibus. Morbi et justo varius, convallis risus ut, rutrum nulla. Morbi suscipit facilisis egestas. Nunc sollicitudin accumsan massa et rutrum. Sed bibendum elit vel vulputate lacinia. Phasellus vel fringilla urna. Morbi rhoncus quis mauris quis sodales. do you satisfied out of my answer?`",
          hasBTN: true,
        });
        this.chatModalForSearch.isCommandInputDisabled = true;
        this.addNewKpiKeyDialogForSearch.description =
          this.chatModalForSearch.userCommand;
        this.chatModalForSearch.userCommand = "";
        return;
      }
      if (this.chatModalForSearch.situation == -1) {
        this.chatModalForSearch.messages.map((x) => (x.hasBTN = false));
        this.chatModalForSearch.messages.push({
          time: new Date(),
          from: "robot",
          message:
            "sorry my bad!, please give me more information so, i can help you better!",
          hasBTN: false,
        });
        this.chatModalForSearch.isCommandInputDisabled = false;
        this.chatModalForSearch.userCommand = "";
        this.chatModalForSearch.situation = 0;
        return;
      }
      if (this.chatModalForSearch.situation == 1) {
        this.chatModalForSearch.messages.map((x) => (x.hasBTN = false));
        this.chatModalForSearch.messages.push({
          time: new Date(),
          from: "robot",
          message: "Yay, now please give me a name as tag for this KPI",
          hasBTN: false,
        });
        this.chatModalForSearch.isCommandInputDisabled = false;
        this.chatModalForSearch.userCommand = "";
        this.chatModalForSearch.situation = 2;
        return;
      }
      if (this.chatModalForSearch.situation == 2) {
        this.chatModalForSearch.isCommandInputDisabled = true;
        this.fireSnack("your new KPI added to the KPIs list for this Asset.");
        this.addNewKpiKeyDialogForSearch.name =
          this.chatModalForSearch.userCommand;
        this.chatModalForSearch.isOpen = false;
        this.chatModalForSearch.situation = 0;
        this.chatModalForSearch.userCommand = "";
        this.chatModalForSearch.messages = [
          {
            time: new Date(),
            from: "robot",
            message:
              "Hello im your AI assistant, please describe me what kind of KPI do you want to add.",
            hasBTN: false,
          },
        ];
        this.addNewKpiToUserKpiListForSearch(null);
      }
    },
    addNewKpiToUserKpiListForSearch(kpiKey) {
      if (
        !this.addNewKpiKeyDialogForSearch.name ||
        !this.addNewKpiKeyDialogForSearch.description
      ) {
        this.fireSnack("all the fields are required!");
        return;
      }
      if (!kpiKey) {
        kpiKey = {
          name: this.addNewKpiKeyDialogForSearch.name,
          data: [],
          description: this.addNewKpiKeyDialogForSearch.description,
        };
      }

      (this.addNewKpiKeyDialogForSearch.type === 0
        ? this.allSavedKPIs
        : this.allSavedAlternativesKPIs
      ).push(kpiKey);
    },
    filterByKPI(isInit = true) {
      if (isInit) this.init();
      this.allTrackingSelectedCompanies =
        this.allTrackingSelectedCompanies.filter(
          (x) => x.score >= this.precisionVsRecall
        );
      if (this.allSelectedKpiItems.length > 0) {
        this.allTrackingSelectedCompanies =
          this.allTrackingSelectedCompanies.filter((x) =>
            x.userKPIs.some((kpi) =>
              this.allSelectedKpiItems.some((z) => z.value === kpi.name)
            )
          );
      }
      if (this.allSelectedAlternativeKpiItems.length > 0)
        this.allTrackingSelectedCompanies =
          this.allTrackingSelectedCompanies.filter((x) =>
            x.userKPIs.some((kpi) =>
              this.allSelectedAlternativeKpiItems.some(
                (z) => z.value === kpi.name
              )
            )
          );
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
    openNewKpiDialogForSearch(kpiType) {
      this.addNewKpiKeyDialogForSearch.type = kpiType;
      this.addNewKpiKeyDialogForSearch.isOpen = true;
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
    hasKpiKey(tag) {
      const foundedKpi = this.addNewKpiKeyDialog.kpiKeysList.find(
        (x) => x.name == tag.name
      );
      if (foundedKpi) return false;
      return true;
    },
    removeKpiToUserKpiList(key) {
      const companyIndex = this.allTrackingSelectedCompanies.findIndex(
        (x) => x.companyId == this.addNewKpiKeyDialog.companyId
      );
      this.allTrackingSelectedCompanies[companyIndex].userKPIs =
        this.allTrackingSelectedCompanies[companyIndex].userKPIs.filter(
          (z) => z.name != key.name
        );

      this.addNewKpiKeyDialog.kpiKeysList =
        this.addNewKpiKeyDialog.kpiKeysList.filter((x) => x.name != key.name);
      api.saveTrackingKPIs(this.allTrackingSelectedCompanies);
      this.fireSnack("KPI key has ben removed from asset.");
      //this.allTrackingSelectedCompanies
    },
    fireSnack(text) {
      this.snackbar.text = text;
      this.snackbar.isOpen = true;
    },
    init() {
      this.allTrackingSelectedCompanies = api.getAllTrackingKPIs();
      this.allSavedKPIs = api.getAllTrackingKPIKeys();
      this.allSavedAlternativesKPIs = api.getAllTrackingKPIKeys();
      this.deleteTrackingDialog = {
        isOpen: false,
        companyId: "",
      };
      this.addNewKpiKeyDialog = {
        isOpen: false,
        name: "",
        description: "",
        companyId: "",
        kpiKeysList: [],
        newSelectedKpiKeys: [],
      };
      this.viewTrackingCompanyDialog.company = null;
      this.viewTrackingCompanyDialog.isOpen = false;
      this.addNewTrackingCompanyDialog.isOpen = false;
      let allAiRobots = api.getAiRobots();
      this.addNewTrackingCompanyDialog.allCompanies = api
        .getStandardCompanyList(api.getCompanies(), allAiRobots)
        .filter(
          (z) =>
            this.allTrackingSelectedCompanies.findIndex(
              (k) => k.companyId == z.companyId
            ) == -1
        );
      this.filterByKPI(false);
    },
    openNewKpiDialog(companyId, kpiType = "userKPIs") {
      this.addNewKpiKeyDialog.companyId = companyId;
      this.addNewKpiKeyDialog.newSelectedKpiKeys = [];
      console.log(
        "api.getAllTrackingAlternativeKPIKeys()",
        api.getAllTrackingAlternativeKPIKeys()
      );
      console.log("api.getAllTrackingKPIKeys()", api.getAllTrackingKPIKeys());
      if (kpiType == "userKPIs") {
        api.getAllTrackingKPIKeys().map((kpi) => {
          this.addNewKpiKeyDialog.newSelectedKpiKeys.push({
            name: kpi.name,
            data: [],
            description: kpi.description,
          });
        });
      } else {
        api.getAllTrackingAlternativeKPIKeys().map((kpi) => {
          this.addNewKpiKeyDialog.newSelectedKpiKeys.push({
            name: kpi.name,
            data: [],
            description: kpi.description,
          });
        });
      }
      let kpiKeys =
        kpiType == "userKPIs"
          ? api.getAllTrackingKPIKeys()
          : api.getAllTrackingAlternativeKPIKeys();
      if (kpiKeys.length == 0)
        kpiKeys =
          kpiType == "userKPIs"
            ? api.getAllTrackingKPIKeys()
            : api.getAllTrackingAlternativeKPIKeys();
      this.addNewKpiKeyDialog.kpiKeysList = kpiKeys;
      this.addNewKpiKeyDialog.kpiType = kpiType;
      this.addNewKpiKeyDialog.isOpen = true;
    },
    showDataSize(dataSize) {
      return api.dataSizeSerializer(dataSize);
    },
    deleteTrackingCompany(companyId) {
      const index = this.allTrackingSelectedCompanies.findIndex(
        (x) => x.companyId == companyId
      );
      this.allTrackingSelectedCompanies.splice(index, 1);
      api.saveTrackingKPIs(this.allTrackingSelectedCompanies);
      this.fireSnack("Company removed from tracking system!");
      this.init();
    },
    viewTrackingCompany(company) {},
    searchChatWithAIBtnSubmitted() {
      if (!this.chatSearchModal.userCommand) {
        this.fireSnack("please write your command first.");
        return;
      }
      this.init();

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
      for (
        let index = 0;
        index < this.allTrackingSelectedCompanies.length / 2;
        index++
      ) {
        const randIndex = Math.floor(
          Math.random() * this.allTrackingSelectedCompanies.length
        );
        this.allTrackingSelectedCompanies.splice(randIndex, 1);
      }
    },

    addCompanyToTrackingSystem() {
      let IndexOfSelectedCompany = -1;
      this.addNewTrackingCompanyDialog.allCompanies.map((company) => {
        if (company.isSelectedAttr) {
          this.allTrackingSelectedCompanies.push(company);
          IndexOfSelectedCompany = this.allTrackingSelectedCompanies.length - 1;
        }
      });
      api.saveTrackingKPIs(this.allTrackingSelectedCompanies);
      this.fireSnack(
        "Company added successfully, now please configure your KPIs for this asset"
      );
      this.init();
      return this.allTrackingSelectedCompanies[IndexOfSelectedCompany];
    },
    changeKPIChart(kpiIndex, kpiType = "userKPIs") {
      if (kpiType == "userKPIs") {
        this.chartSeries = [
          {
            name: this.viewTrackingCompanyDialog.company?.userKPIs[kpiIndex]
              .name,
            data: this.viewTrackingCompanyDialog.company?.userKPIs[
              kpiIndex
            ].data.map((x) => x.value),
          },
        ];
        this.chartOptions = {
          chart: {
            id: "line-chart",
          },
          xaxis: {
            categories: this.viewTrackingCompanyDialog.company?.userKPIs[
              kpiIndex
            ].data.map((x) => x.date), // assuming item.time is an array of time labels
          },
        };
      }
      if (kpiType == "userAlternativeKPIs") {
        this.chartSeries = [
          {
            name: this.viewTrackingCompanyDialog.company?.userAlternativeKPIs[
              kpiIndex
            ].name,
            data: this.viewTrackingCompanyDialog.company?.userAlternativeKPIs[
              kpiIndex
            ].data.map((x) => x.value),
          },
        ];
        this.chartOptions = {
          chart: {
            id: "line-chart",
          },
          xaxis: {
            categories:
              this.viewTrackingCompanyDialog.company?.userAlternativeKPIs[
                kpiIndex
              ].data.map((x) => x.date), // assuming item.time is an array of time labels
          },
        };
      }
    },
    clickOnRobotChat() {
      document.getElementById("btnStartConversation").click();
    },
    addNewKpiKeyToUserKpiKeyList() {},
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
      if (this.addNewKpiKeyDialog.kpiType == "userAlternativeKPIs") {
        this.addNewKpiKeyDialog.newSelectedKpiKeys.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });
        api.saveTrackingAlternativeKPIKeys(
          this.addNewKpiKeyDialog.newSelectedKpiKeys
        );
        this.addNewKpiKeyDialog.kpiKeysList.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });

        const allAiRobots = api.getAllAiRobots();
        const companyIndex = this.allTrackingSelectedCompanies.findIndex(
          (x) => x.companyId == this.addNewKpiKeyDialog.companyId
        );
        this.allTrackingSelectedCompanies[
          companyIndex
        ].userAlternativeKPIs.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });
        this.allTrackingSelectedCompanies = api.getStandardCompanyList(
          this.allTrackingSelectedCompanies,
          allAiRobots
        );
        api.saveTrackingKPIs(this.allTrackingSelectedCompanies);
        this.addNewKpiKeyDialog.name = "";
        this.addNewKpiKeyDialog.description = "";
        this.addNewKpiKeyDialog.kpiType = "userAlternativeKPIs";
        this.fireSnack("Alternative KPI Key added successfully!");
      } else {
        this.addNewKpiKeyDialog.newSelectedKpiKeys.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });
        api.saveTrackingKPIKeys(this.addNewKpiKeyDialog.newSelectedKpiKeys);
        this.addNewKpiKeyDialog.kpiKeysList.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });

        const allAiRobots = api.getAllAiRobots();
        const companyIndex = this.allTrackingSelectedCompanies.findIndex(
          (x) => x.companyId == this.addNewKpiKeyDialog.companyId
        );
        this.allTrackingSelectedCompanies[companyIndex].userKPIs.push({
          name: kpiKey.name,
          data: [],
          description: kpiKey.description,
        });
        this.allTrackingSelectedCompanies = api.getStandardCompanyList(
          this.allTrackingSelectedCompanies,
          allAiRobots
        );
        api.saveTrackingKPIs(this.allTrackingSelectedCompanies);
        this.addNewKpiKeyDialog.name = "";
        this.addNewKpiKeyDialog.description = "";
        this.fireSnack("KPI Key added successfully!");
      }
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
.cart-track {
  padding: 30px !important;
  color: black !important;
  background: linear-gradient(
    135deg,
    rgba(251, 255, 252, 1) 0%,
    rgb(218, 178, 255) 100%
  );
}
</style>
