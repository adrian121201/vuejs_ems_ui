<template>
  <v-container fluid>
    <div class="mb-4">
      <v-btn color="#73AB6B" style="color: white;" @click="dialog = true">Add Record</v-btn>
    </div>
    <div>
      <v-card elevation="0">
        <v-card-title>
          EMPLOYEE LIST
          <v-spacer></v-spacer>
          <v-spacer></v-spacer>
          <v-spacer></v-spacer>
          <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line
            hide-details></v-text-field>
        </v-card-title>
        <v-data-table :items="employeeList" :headers="headers" :search="search">
          <!-- <template v-slot:item.actions="{ item }"></template> -->

          <template slot="item.actions" scope="{ item }">
            <v-btn icon color="primary" @click="viewData(item)"><v-icon>mdi-open-in-new</v-icon></v-btn>
            <v-btn icon color="warning" @click="getUpdateData(item)"><v-icon>mdi-pencil</v-icon> </v-btn>
            <v-btn icon color="error" @click="deleteRecordDialog(item.id)"><v-icon>mdi-delete</v-icon> </v-btn>
          </template>
        </v-data-table>
      </v-card>
    </div>

    <!-- ADD RECORD DIALOG BOX -->
    <v-dialog persistent v-model="dialog" width="400">
      <v-card>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field v-model="employee_number" :rules="EmployeeNumberRules" label="Employee Number"></v-text-field>

            <v-text-field v-model="last_name" :rules="LastNameRules" label="Last Name"></v-text-field>

            <v-text-field v-model="first_name" :rules="LastNameRules" label="First Name"></v-text-field>

            <v-text-field v-model="middle_name" :rules="MiddleNameRules" label="Middle Name">
            </v-text-field>

            <v-select v-model="Department" :items="departmentOptions" :rules="DepartmentRules" label="Department">
            </v-select>

            <v-select v-model="employee_type" :items="employeeTypeOptions" :rules="EmployeeTypeRules"
              label="Employee Type">
            </v-select>

          </v-form>
        </v-card-text>
        <v-card-actions class="pb-5">
          <v-btn color="primary" @click="addRecord">Add Record</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="error" @click="reset">
            Reset Form
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="error" @click="dialog = false">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- VIEW DIALOG BOX -->
    <v-dialog persistent v-model="viewDialog" width="500">
      <v-card>
        <v-toolbar dense color="primary" class="mb-2" dark>
          <v-icon class="mr-2">mdi-menu</v-icon>
          <v-toolbar-title>{{ first_name }} {{ middle_name }} {{ last_name }}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon small @click="viewDialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text>
          <v-list dense>
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
                <v-list-item-title>Last Name</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ last_name }}</v-list-item-subtitle>
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
                <v-list-item-title>Middle Name</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ middle_name }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Department</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ Department }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>Employee Type</v-list-item-title>
              </v-list-item-content>
              <v-list-item-content>
                <v-list-item-subtitle>: {{ employee_type }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="getUpdateData(update_record)">
            <v-icon small left>mdi-pencil</v-icon>
            Edit
          </v-btn>

          <v-btn color="error" @click="deleteRecordDialog(employee_id)">
            <v-icon small left>mdi-delete</v-icon>
            Delete
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- UPDATE RECORD DIALOG BOX -->
    <v-dialog persistent v-model="updateDialog" width="400">
      <v-card>
        <v-toolbar dense color="primary" class="mb-2" dark>
          <v-icon class="mr-2">mdi-menu</v-icon>
          <v-toolbar-title>Update Record</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon small @click="updateDialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field v-model="employee_number" :rules="EmployeeNumberRules" label="Employee Number"></v-text-field>

            <v-text-field v-model="last_name" :rules="LastNameRules" label="Last Name"></v-text-field>

            <v-text-field v-model="first_name" :rules="FirstNameRules" label="First Name"></v-text-field>

            <v-text-field v-model="middle_name" :rules="MiddleNameRules" label="Middle Name">
            </v-text-field>

            <v-select v-model="Department" :items="departmentOptions" :rules="DepartmentRules" label="Department">
            </v-select>

            <v-select v-model="employee_type" :items="employeeTypeOptions" :rules="EmployeeTypeRules"
              label="Employee Type">
            </v-select>

          </v-form>
        </v-card-text>
        <v-card-actions class="pb-5">
          <v-btn color="primary" @click="updateRecord">Update Record</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="error" @click="dialogReset">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


    <!-- DELETE DIALOG BOX -->
    <v-dialog persistent v-model="deleteDialog" width="300">
      <v-sheet class="px-7 pt-7 pb-4 mx-auto text-center d-inline-block" color="blue-grey darken-3" dark>
        <div class="grey--text text--lighten-1 text-body-2 mb-4">
          Are you sure you want to delete this record?
        </div>

        <v-btn class="ma-1" color="grey" plain @click="deleteDialog = false">
          Cancel
        </v-btn>

        <v-btn class="ma-1" color="error" plain @click="deleteRecord">
          Delete
        </v-btn>
      </v-sheet>
    </v-dialog>

    <!-- SNACKBAR -->
    <!-- Add Record snackbar-->
    <v-snackbar v-model="snackbar" color="success" top right>
      Successfully Created!

      <template v-slot:action="{ attrs }">
        <v-btn icon v-bind="attrs" @click="snackbar = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>

    <!-- update record snackbar -->
    <v-snackbar v-model="updateSnackbar" color="green" top right>
      Successfully Updated!

      <template v-slot:action="{ attrs }">
        <v-btn icon v-bind="attrs" @click="updateSnackbar = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>

    <!-- Delete record snackbar -->
    <v-snackbar v-model="deleteSnackbar" color="green" top right>
      Successfully Deleted Record!

      <template v-slot:action="{ attrs }">
        <v-btn icon v-bind="attrs" @click="deleteSnackbar = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
export default {
  name: "InspirePage",
  data() {
    return {
      snackbar: false,
      deleteSnackbar: false,
      updateSnackbar: false,
      valid: true,
      employee_id: null,
      employee_number: "",
      last_name: "",
      first_name: "",
      middle_name: "",
      Department: "",
      employee_type: "",
      dialog: false,
      viewDialog: false,
      deleteDialog: false,
      updateDialog: false,
      search: "",

      Department: '', // This will hold the selected value
      DepartmentRules: [(v) => !!v || "Department is required"],
      departmentOptions: [
        'IT',
        'Computer',
        'Comp Scie',
        'Comp Engineering',
        'CSS',
        'Graphics',
        'Technical'
      ],

      employee_type: '', // This will hold the selected value for the dropdown
      EmployeeTypeRules: [(v) => !!v || "Employee Type is required"],
      employeeTypeOptions: [
        'Regular',
        'Full-Time',
        'Part-Time',
        'Contractual'
      ],


      headers: [
        {
          text: "Employee Number",
          align: "start",
          sortable: false,
          value: "attributes.employee_number",
        },
        {
          text: "Last Name",
          value: "attributes.last_name",
          sortable: false,
        },
        {
          text: "First Name",
          value: "attributes.first_name",
          sortable: false,
        },
        {
          text: "Middle Name",
          value: "attributes.middle_name",
          sortable: false,
        },
        {
          text: "Department",
          value: "attributes.Department",
          sortable: false,
        },
        {
          text: "Employee Type",
          value: "attributes.employee_type",
          sortable: false,
        },
        { text: "", value: "actions", align: "end" },
      ],
      EmployeeNumberRules: [(v) => !!v || "Employee Number is required"],
      LastNameRules: [(v) => !!v || "Last Name is required"],
      FirstNameRules: [(v) => !!v || "First Name Description is required"],
      MiddleNameRules: [(v) => !!v || "Middle Name is required"],
      DepartmentRules: [(v) => !!v || "Department is required"],
      EmployeeTypeRules: [(v) => !!v || "Employee Type is required"],

      employeeList: [],
      update_record: {}
    };
  },

  mounted() {
    this.getEmployeeList();
  },

  methods: {

    dialogReset(){
     
      this.$refs.form.reset()
      this.updateDialog = false
    

    },


    reset() {
      this.$refs.form.reset()
    },

    getEmployeeList() {
      this.$axios
        .get("/api/employeelist-tables")
        .then((response) => {
          console.log(response);
          this.employeeList = response.data.data;
        })
        .catch((error) => {
          console.log("error");
        });
    },

    viewData(item) {
      console.log(item);
      this.update_record = item;
      this.employee_id = item.id;
      this.employee_number = item.attributes.employee_number;
      this.last_name = item.attributes.last_name;
      this.first_name = item.attributes.first_name;
      this.middle_name = item.attributes.middle_name;
      this.Department = item.attributes.Department;
      this.employee_type = item.attributes.employee_type;
      this.viewDialog = true;
    },

    getUpdateData(item) {

      this.employee_id = item.id;
      this.employee_number = item.attributes.employee_number;
      this.last_name = item.attributes.last_name;
      this.first_name = item.attributes.first_name;
      this.middle_name = item.attributes.middle_name;
      this.Department = item.attributes.Department;
      this.employee_type = item.attributes.employee_type;
      this.updateDialog = true;
    },


    addRecord() {
      if (this.$refs.form.validate()) {
        let payload = {
          data: {
            employee_number: this.employee_number,
            last_name: this.last_name,
            first_name: this.first_name,
            middle_name: this.middle_name,
            Department: this.Department,
            employee_type: this.employee_type,
          },
        };
        this.$axios
          .post("/api/employeelist-tables", payload)
          .then((response) => {
            console.log(response);
            this.$refs.form.reset();
            this.getEmployeeList();
            this.snackbar = true;

          })
          .catch((error) => {
            console.log(error);
          });
      }
    },

    updateRecord() {
      if (this.$refs.form.validate()) {
        let payload = {
          data: {
            employee_number: this.employee_number,
            last_name: this.last_name,
            first_name: this.first_name,
            middle_name: this.middle_name,
            Department: this.Department,
            employee_type: this.employee_type,
          },
        };

        this.$axios
          .put(`/api/employeelist-tables/${this.employee_id}`, payload)
          .then((response) => {
            console.log(response);
            //this.$refs.form.reset();
            this.getEmployeeList();
            this.updateSnackbar = true;

          })
          .catch((error) => {
            console.log(error);
          });
      }
    },

    deleteRecordDialog(id) {
      console.log(id)
      this.employee_id = id;
      this.deleteDialog = true;
    },

    deleteRecord() {
      this.$axios.delete(`/api/employeelist-tables/${this.employee_id}`)
        .then(response => {
          console.log(response);
          this.deleteSnackbar = true;
          this.getEmployeeList();
          this.deleteDialog = false
          this.viewDialog = false;
        })
        .catch(error => {
          console.log(error);
        })
    }



  },

  computed: {},
};
</script>