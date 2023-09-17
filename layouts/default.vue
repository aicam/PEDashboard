<template>
  <v-app dark>
    <v-btn
      id="btnStartConversation"
      style="
        position: fixed;
        z-index: 9;
        right: 1vw;
        bottom: 1vh;
        background-color: white;
      "
      color="primary"
      elevation="6"
      icon
      outlined
      plain
      text
      x-large
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
    >
      <v-icon color="green"> mdi-robot </v-icon></v-btn
    >
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

    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :href="item.to"
          router
          exact
          replace
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer v-model="rightDrawer" :right="right" temporary fixed>
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light> mdi-repeat </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  methods: {
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
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        // {
        //   icon: "mdi-apps",
        //   title: "Welcome",
        //   to: "/",
        // },
        {
          icon: "mdi-monitor-dashboard",
          title: "Dashboard",
          to: "/dashboard",
        },
        {
          icon: "mdi-shield-check",
          title: "Recommendations",
          to: "/dashboard/recommendations",
        },
        {
          icon: "mdi-radar",
          title: "Track",
          to: "/dashboard/tracking",
        },
        {
          icon: "mdi-head-lightbulb",
          title: "Insight",
          to: "/dashboard/deal_insights",
        },
        {
          icon: "mdi-text-box-search",
          title: "Data",
          to: "/dashboard/dataCollector",
        },
        // {
        //   icon: "mdi-handshake",
        //   title: "Deal Sourcing",
        //   to: "/deal_sourcing",
        // },
        {
          icon: "mdi-projector-screen-variant-outline",
          title: "Models Trained",
          to: "/dashboard/modelsTrained",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Vuetify.js",
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
    };
  },
};
</script>
