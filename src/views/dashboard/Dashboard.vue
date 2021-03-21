<template>
  <v-container
    id="dashboard"
    fluid
    tag="section"
  >
  <div class="text-center">
    <v-btn
      dark
      color="red darken-2"
      @click="sendEmail()"
    >
      Suspicious traffic
    </v-btn> &nbsp;
    <v-btn
      dark
      color="red darken-2"
      @click="latency()"
    >
      Degrade Latency
    </v-btn> &nbsp;
    <v-btn
      dark
      color="red darken-2"
      @click="performance()"
    >
      Degrade network Performance
    </v-btn>

    <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
    >
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="latencyReverse()"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-snackbar
      v-model="snackbar2"
      :multi-line="multiLine"
    >
      {{ text2 }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="performanceReverse()"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-snackbar
      v-model="snackbar3"
      :multi-line="multiLine"
    >
      {{ text3 }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="red"
          text
          v-bind="attrs"
          @click="snackbar3 = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </div>
    <v-row>
      <v-col
        cols="12"
        lg="12"
      >
        <base-material-chart-card
          :data="dataCompletedTasksChart.data"
          :options="dataCompletedTasksChart.options"
          hover-reveal
          color="info"
          type="Line"
        >
          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h3 class="card-title font-weight-light mt-2 ml-2">
            Network Performance
          </h3>

          <p class="d-inline-flex font-weight-light ml-2 mt-1">
            Last 12 hours
          </p>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="green"
          icon="mdi-network"
          title="PING ms"
          :value="ping"
          sub-icon="mdi-clock"
          sub-text="Just Updated"
        />
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="cyan"
          icon="mdi-download"
          title="DOWNLOAD Mbps"
          :value="downSpeed"
          sub-icon="mdi-clock"
          sub-text="Just Updated"
        />
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="3"
      >
        <base-material-stats-card
          color="purple"
          icon="mdi-upload"
          title="UPLOAD Mbps"
          :value="upSpeed"
          sub-icon="mdi-clock"
          sub-text="Just Updated"
        />
      </v-col>

      <v-col
        cols="12"
        md="6"
      >
        <base-material-card
          color="warning"
          class="px-5 py-3"
        >
          <template v-slot:heading>
            <v-tabs
              v-model="tabs"
              background-color="transparent"
              slider-color="white"
            >
              <v-tab>
                <v-icon class="mr-2">
                  mdi-cloud
                </v-icon>
                Network Devices
              </v-tab>
            </v-tabs>
          </template>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="items"
            />
          </v-card-text>
        </base-material-card>
      </v-col>

    </v-row>
  </v-container>
</template>
<script>
  import emailjs from 'emailjs-com'
  export default {
    name: 'DashboardDashboard',

    data () {
      return {
        ping: '3',
        downSpeed: '24',
        upSpeed: '25',
        multiLine: true,
        snackbar: false,
        snackbar2: false,
        snackbar3: false,
        name: 'Yogita',
        from: 'Network Tool',
        message: 'We found a suspicious traffic in your network',
        latencyMessage: 'Dear user, your network latency has been degraded.',
        performanceMessage: 'Dear user, your network performance is not in a good state.',
        reply: 'akashpawara29@gmail.com',
        text: 'Latency is degrading',
        text2: 'performance is degrading',
        text3: 'Suspicious traffic detected',
        dailySalesChart: {
          data: {
            labels: ['M', 'T', 'W', 'T', 'F', 'S', 'S'],
            series: [
              [12, 17, 7, 17, 23, 18, 38],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
            chartPadding: {
              top: 0,
              right: 0,
              bottom: 0,
              left: 0,
            },
          },
        },
        dataCompletedTasksChart: {
          data: {
            labels: ['12am', '3pm', '6pm', '9pm', '12pm', '3am', '6am', '9am'],
            series: [
              [30, 150, 50, 200, 80, 40, 20, 90],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 300, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
            chartPadding: {
              top: 0,
              right: 0,
              bottom: 0,
              left: 0,
            },
          },
        },
        emailsSubscriptionChart: {
          data: {
            labels: ['Ja', 'Fe', 'Ma', 'Ap', 'Mai', 'Ju', 'Jul', 'Au', 'Se', 'Oc', 'No', 'De'],
            series: [
              [542, 443, 320, 780, 553, 453, 326, 434, 568, 610, 756, 895],

            ],
          },
          options: {
            axisX: {
              showGrid: false,
            },
            low: 0,
            high: 1000,
            chartPadding: {
              top: 0,
              right: 5,
              bottom: 0,
              left: 0,
            },
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc: function (value) {
                  return value[0]
                },
              },
            }],
          ],
        },
        headers: [
          {
            sortable: false,
            text: 'ID',
            value: 'id',
          },
          {
            sortable: false,
            text: 'Name',
            value: 'name',
          },
          {
            sortable: false,
            text: 'Description',
            value: 'description',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Flags',
            value: 'flags',
            align: 'right',
          },
        ],
        items: [
          {
            id: 1,
            name: 'wlp3s0',
            description: '',
            flags: '',
          },
          {
            id: 2,
            name: 'lo',
            description: '',
            flags: 'PCAP_IF_LOOPBACK',
          },
          {
            id: 3,
            name: 'any',
            description: 'Pseudo-device that captures on all interfaces',
            flags: '',
          },
          {
            id: 4,
            name: 'enp2s0',
            description: '',
            flags: '',
          },
          {
            id: 5,
            name: 'docker0',
            description: '',
            flags: '',
          },
          {
            id: 6,
            name: 'bluetooth-monitor',
            description: 'Bluetooth Monitor',
            flags: '',
          },
          {
            id: 7,
            name: 'nflog',
            description: 'Netfilter log (NFLOG) interface',
            flags: '',
          },
          {
            id: 8,
            name: 'nfqueue',
            description: 'Netfilter queue (NFQUEUE) interface',
            flags: '',
          },
          {
            id: 9,
            name: 'dbus-system',
            description: 'D-Bus system bus',
            flags: '',
          },
          {
            id: 10,
            name: 'dbus-session',
            description: 'D-Bus session bus',
            flags: '',
          },
        ],
        tabs: 0,
        tasks: {
          0: [
            {
              text: 'wlp3s0',
              value: 'nill',
            },
            {
              text: 'lo',
              value: 'PCAP_IF_LOOPBACK',
            },
            {
              text: 'any',
              value: 'Pseudo-device that captures on all interfaces',
            },
            {
              text: 'enp2s0',
              value: 'nill',
            },
            {
              text: 'docker0',
              value: 'nill',
            },
            {
              text: 'bluetooth-monitor',
              value: 'Bluetooth Linux Monitor',
            },
            {
              text: 'nflog',
              value: 'Linux netfilter log (NFLOG) interface',
            },
            {
              text: 'nfqueue',
              value: 'Linux netfilter queue (NFQUEUE) interface',
            },
            {
              text: 'dbus-system',
              value: 'D-Bus system bus',
            },
            {
              text: 'dbus-session',
              value: 'D-Bus session bus',
            },
          ],
          1: [
            {
              text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              value: true,
            },
            {
              text: 'Sign contract for "What are conference organizers afraid of?"',
              value: false,
            },
          ],
          2: [
            {
              text: 'Lines From Great Russian Literature? Or E-mails From My Boss?',
              value: false,
            },
            {
              text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              value: true,
            },
            {
              text: 'Sign contract for "What are conference organizers afraid of?"',
              value: true,
            },
          ],
        },
        list: {
          0: false,
          1: false,
          2: false,
        },
      }
    },

    methods: {
      complete (index) {
        this.list[index] = !this.list[index]
      },
      sendEmail (e) {
        var templateParams = {
          name: this.name,
          from: this.from,
          message: this.message,
          reply: this.reply,
        }
        try {
          emailjs.send('gmail', 'template_4tvt50q', templateParams, 'user_WSNz5jglkfmXSr40i2BXa')
        } catch (error) {
          console.log({ error })
        }
        this.snackbar3 = true
      },
      latency (e) {
        this.snackbar = true
        this.ping = '150'
        this.downSpeed = '2'
        this.upSpeed = '1'
        var templateParams = {
          name: this.name,
          from: this.from,
          message: this.latencyMessage,
          reply: this.reply,
        }
        try {
          emailjs.send('gmail', 'template_4tvt50q', templateParams, 'user_WSNz5jglkfmXSr40i2BXa')
        } catch (error) {
          console.log({ error })
        }
      },
      latencyReverse (e) {
        this.snackbar = false
        this.ping = '5'
        this.downSpeed = '23'
        this.upSpeed = '24'
      },
      performance (e) {
        this.snackbar2 = true
        this.dataCompletedTasksChart.data.series = [[30, 150, 50, 200, 280, 240, 220, 290]]
        var templateParams = {
          name: this.name,
          from: this.from,
          message: this.performanceMessage,
          reply: this.reply,
        }
        try {
          emailjs.send('gmail', 'template_4tvt50q', templateParams, 'user_WSNz5jglkfmXSr40i2BXa')
        } catch (error) {
          console.log({ error })
        }
      },
      performanceReverse (e) {
        this.snackbar2 = false
        this.dataCompletedTasksChart.data.series = [[30, 150, 50, 200, 80, 40, 20, 90]]
      },
    },
  }
</script>
