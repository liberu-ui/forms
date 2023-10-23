<template>
  <div>
<!--    <v-dialog
        ref="dialog"
        v-model="dialogModel"
        :return-value.sync="field.value"
        width="290px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
            v-model="field.value"
            :label="field.label"
            prepend-icon="mdi-calendar"
            readonly
            v-bind="attrs"
            v-on="on"
            @click="triggerDialog(true)"
        ></v-text-field>
      </template>
      <v-locale-provider :locale="locale">
      <v-date-picker
          v-model="field.value"
          scrollable
      >
        <v-spacer></v-spacer>
        <v-btn
            text
            color="primary"
            @click="triggerDialog(false)"
        >
          Cancel
        </v-btn>
        <v-btn
            text
            color="primary"
            @click="$refs.dialog.save(field.value)"
        >
          OK
        </v-btn>
      </v-date-picker>
      </v-locale-provider>
    </v-dialog>-->
<!--    <div class="text-center">
      <v-btn
          color="primary"
          @click="dialog = true"
      >
        Open Dialog
      </v-btn>

      <v-dialog
          v-model="dialog"
          width="auto"
      >
        <v-card>
          <v-card-text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" block @click="dialog = false">Close Dialog</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>-->

    <v-menu
        ref="dateMenu"
        v-model="dialogModel"
        :return-value.sync="field.value"
        :close-on-content-click="false"
        transition="scale-transition"
        offset-y
        min-width="auto"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-text-field
            name="dob"
            id="bio_dob"
            v-model="field.value"
            append-icon="mdi-calendar"
            v-bind="attrs"
            v-on="on"
            filled
            rounded
            dense
            class="rounded-0"
            @click="triggerDialog(true)"
        ></v-text-field>
      </template>

      <v-locale-provider :locale="locale">
        <v-date-picker
            v-model="field.value"
            scrollable
        >
          <v-spacer></v-spacer>
          <v-btn
              text
              color="primary"
              @click="triggerDialog(false)"
          >
            Cancel
          </v-btn>
          <v-btn
              text
              color="primary"
              @click="$refs.dateMenu.save(field.value); triggerDialog(false)"
          >
            OK
          </v-btn>
        </v-date-picker>
      </v-locale-provider>
    </v-menu>
  </div>
<!--  <v-locale-provider :locale="locale">
    <v-date-picker></v-date-picker>
  </v-locale-provider>-->
</template>

<script setup>
import {ref} from "vue";
import { VDatePicker } from 'vuetify/labs/VDatePicker';

defineProps(
    {
      errors: {
        type: Object,
        required: true,
      },
      field: {
        type: Object,
        required: true,
      },
      i18n: {
        type: Function,
        required: true,
      },
      locale: {
        type: String,
        required: true,
      },
      timeOnly: {
        type: Boolean,
        default: false,
      },
    }
);

defineEmits(['change'])

const dialogModel = ref(false)
const dialog = ref(false)
const dobMenu = ref(false)
const dob = ref(null)

function triggerDialog(val) {
  dialogModel.value = val
}





/*export default {
    name: 'DateField',

    components: { Datepicker },

    props: {
        errors: {
            type: Object,
            required: true,
        },
        field: {
            type: Object,
            required: true,
        },
        i18n: {
            type: Function,
            required: true,
        },
        locale: {
            type: String,
            required: true,
        },
        timeOnly: {
            type: Boolean,
            default: false,
        },
    },

    emits: ['changed'],

    methods: {
        clear() {
            this.$refs.datepicker.clear();
        },
    },
};*/
</script>
