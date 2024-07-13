<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" sm="3" md="3">
        <DashboardCard :title="`Total Employees`" :value="38" :color="`green`" :icon="`mdi-account-group`" />
      </v-col>
      <v-col cols="12" sm="3" md="3">
        <DashboardCard :title="`Reports`" :value="9" :color="`orange`" :icon="`mdi-chart-box-outline`" />
      </v-col>
      <v-col cols="12" sm="3" md="3">
        <DashboardCard :title="`Messages`" :value="5" :color="`red`" :icon="`mdi-message-badge-outline`" />
      </v-col>
      <v-col cols="12" sm="3" md="3">
        <DashboardCard :title="`Leaves`" :value="3" :color="`blue`" :icon="`mdi-account-arrow-left-outline`" />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="4">
        <v-row class="mt-2">
          <v-col cols="12">
            <v-card class="mt-4 mx-auto" elevation="0">
              <v-sheet class="v-sheet--offset mx-auto" color="cyan" elevation="12" max-width="calc(100% - 32px)">
                <v-sparkline :labels="labels" :value="dataValues" color="white" line-width="2" padding="16"></v-sparkline>
              </v-sheet>
              <v-card-text class="pt-0">
                <div class="text-h6 font-weight-light mb-2">
                  Working Status
                </div>
                <div class="subheading font-weight-light grey--text">
                  Last Campaign Performance
                </div>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="12">
            <v-card class="mt-4 mx-auto" elevation="0">
              <v-sheet class="v-sheet--offset mx-auto" color="warning" elevation="12" max-width="calc(100% - 32px)">
                <v-sparkline :labels="labels" :value="dataValues" color="white" line-width="2" padding="16"></v-sparkline>
              </v-sheet>

              <v-card-text class="pt-0">
                <div class="text-h6 font-weight-light mb-2">
                  Daily Collections
                </div>
                <div class="subheading font-weight-light grey--text">
                  Last Campaign Performance
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="12" md="8">
        <v-card elevation="0">
          <div style="margin-top: 11px;">
            <v-progress-linear indeterminate color="yellow darken-2"></v-progress-linear>
            <br>
            <v-progress-linear indeterminate color="green"></v-progress-linear>
            <br>
            <v-progress-linear indeterminate color="teal"></v-progress-linear>
            <br>
            <v-progress-linear indeterminate color="cyan"></v-progress-linear>
          </div>
         </v-card>
        <v-card>
          <div class="text-center" style="margin-top: 20px;">
            <v-row justify="center">
              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="360" :size="100" :width="15" :value="progressValue" color="teal">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2" style="color: #424242;">Employment</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="-90" :size="100" :width="15" :value="progressValue" color="primary">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2" style="color: #424242;">Status</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="90" :size="100" :width="15" :value="progressValue" color="red">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2" style="color: #424242;">Resignation</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="180" :size="100" :width="15" :value="progressValue" color="pink">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2" style="color: #424242;">Complaints</div>
              </v-col>
            </v-row>
          </div>
        </v-card>
        <v-card>
          <div class="text-center" style="margin-top: 20px;">
            <v-row justify="center">
              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="360" :size="100" :width="15" :value="progressValue" color="teal">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2"style="color: #424242;">Working</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="-90" :size="100" :width="15" :value="progressValue" color="primary">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2"style="color: #424242;">Reports</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="90" :size="100" :width="15" :value="progressValue" color="red">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2"style="color: #424242;">Sales</div>
              </v-col>

              <v-col cols="6" md="3" class="d-flex flex-column align-items-center">
                <v-progress-circular :rotate="180" :size="100" :width="15" :value="progressValue" color="pink">
                  {{ progressValue }}
                </v-progress-circular>
                <div class="mt-2"style="color: #424242;">Revenue</div>
              </v-col>
            </v-row>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import DashboardCard from "../components/DashboardCard.vue";

export default {
  components: {
    DashboardCard,
  },
  data() {
    return {
      interval: null, // Initialize interval as null
      progressValue: 0, // Changed name to avoid conflict with the array 'value'
      labels: ["12am", "3am", "6am", "9am", "12pm", "3pm", "6pm", "9pm"],
      dataValues: [200, 675, 410, 390, 310, 460, 250, 240], // Changed name to avoid conflict with 'progressValue'
    };
  },
  beforeUnmount() {
    clearInterval(this.interval);
  },
  mounted() {
    this.interval = setInterval(() => {
      if (this.progressValue >= 100) {
        this.progressValue = 0;
      } else {
        this.progressValue += 10;
      }
    }, 1000);
  },
};
</script>

<style scoped>
.v-sheet--offset {
  top: -24px;
  position: relative;
}

.v-progress-circular {
  margin: 1rem;
}
</style>
