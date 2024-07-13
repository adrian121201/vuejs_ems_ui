<template>
  <v-container fluid>
    <div>
      <v-card elevation="0">
        <v-card-title>
          EMPLOYEE'S ACCOUNT
          <v-spacer></v-spacer>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table :items="employeeData" :headers="headers" :search="search">
          <template slot="item.actions" scope="{ item }">
            <v-btn icon color="primary" @click="viewData(item)">
              <v-icon>mdi-open-in-new</v-icon>
            </v-btn>
            <v-btn icon color="warning">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon color="error">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-card>
    </div>

    <!-- DIALOG BOX -->
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-toolbar dense color="primary" class="mb-2" dark>
          <v-icon class="mr-2">mdi-account-circle</v-icon>
          <v-toolbar-title>Employee Details</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon small @click="dialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text>
          <v-list dense>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>ID</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ id }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Employee Number</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ employee_number }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>First Name</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ first_name }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Last Name</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ last_name }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Middle Initial</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ middle_initial }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Email</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ email }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary">
            <v-icon small left>mdi-pencil</v-icon>
            Edit
          </v-btn>
          <v-btn color="error">
            <v-icon small left>mdi-delete</v-icon>
            Delete
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      search: "",
      headers: [
        { text: "ID", align: "start", sortable: false, value: "id" },
        { text: "Employee Number", value: "employee_number", sortable: false },
        { text: "First Name", value: "first_name", sortable: false },
        { text: "Last Name", value: "last_name", sortable: false },
        { text: "Email", value: "email", sortable: false },
        { text: "", value: "actions", align: "end" },
      ],
      employeeData: [
        {
          id: 1,
          employee_number: "12-002",
          first_name: "Adrian",
          last_name: "Icalina",
          middle_initial: "A.",
          email: "adrianicalina21@gmail.com",
        },
        {
          id: 2,
          employee_number: "12-003",
          first_name: "Rolly",
          last_name: "Arellano",
          middle_initial: "B",
          email: "arellanorolly@gmail.com",
        },
        {
          id: 3,
          employee_number: "13-890",
          first_name: "Jerdy",
          last_name: "Esguerra",
          middle_initial: "C",
          email: "esguerrajrdy@gmail.com",
        },
      ],

      // Fields for the dialog
      id: null,
      employee_number: "",
      first_name: "",
      last_name: "",
      middle_initial: "",
      email: "",
    };
  },

  methods: {
    viewData(item) {
      this.id = item.id;
      this.employee_number = item.employee_number;
      this.first_name = item.first_name;
      this.last_name = item.last_name;
      this.middle_initial = item.middle_initial;
      this.email = item.email;
      this.dialog = true;
    },
  },
};
</script>
