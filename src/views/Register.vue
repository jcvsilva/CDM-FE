<template>
  <div class="register">
    <Spinner v-if="isLoading" />
    <div v-else>
      <h1>Registers for project {{ route.params.project_number }}</h1>
      <table class="register_table">
        <tr>
          <!-- 6 colunas -->
          <th v-for="header in headers" :key="header">{{ header }}</th>
          <!-- <th>Registers</th>
          <th>Description of risk or hazard</th>
          <th>Risk status</th>
          <th>Discipline</th>
          <th>Risk number (unmitigated) (calculated)</th>
          <th>Revision</th>
          <th>Project lifecycle stage</th>
          <th>HS risk</th>
          <th>Environmental risk</th>
          <th>Programme risk</th>
          <th>Other risk</th>
          <th>Likelihood</th>
          <th>Severity</th>
          <th>Risk product (calculated)</th>
          <th>Relevant documentation</th>
          <th>Owner of risk</th>
          <th>Mitigation action</th>
          <th>Likelihood mitigated</th>
          <th>Severity mitigation</th>
          <th>Risk product (Mitigated) (calculated)</th>
          <th>Further action required</th>
          <th>Continuation Risk reference (calculated)</th>
          <th>Identified by</th>
          <th>Date</th> -->
        </tr>

        <!-- Call Single Register Component with a for loop -->
        <SingleRegister
          v-for="register in registers"
          :key="register.register_id"
          :register="register"
          @reloadregisters="reloadregisters"
          :disciplineList="disciplineList"
          :One2FiveList="One2FiveList"
        />
        <label for="add_button">Add new risk:</label>
        <button form="new_risk" id="add_button" @click="handleClickAdd">
          ➕
        </button>

        <!-- Add new line -->
        <!-- <SingleRegisterAdd
          @reloadregisters="reloadregisters"
          :disciplineList="disciplineList"
          :One2FiveList="One2FiveList"
        /> -->
      </table>
    </div>
  </div>
</template>

<script>
import Registers from "@/composables/Registers";
import SingleRegister from "@/components/SingleRegister";
import Spinner from "@/components/Spinner";
import { useRoute } from "vue-router";
import { ref } from "vue";
import router from "../router";

export default {
  components: { SingleRegister, Spinner },
  setup() {
    const route = useRoute();
    const isLoading = ref(false);

    const disciplineList = [
      "GEN",
      "MULTI",
      "OTHER",
      "ACOU",
      "ARCH",
      "CIV",
      "ELEC",
      "FACM",
      "FCDE",
      "FIN",
      "FIRE",
      "M&E",
      "MECH",
      "PM",
      "R-3R",
      "R-OLE",
      "R-PH",
      "R-PS",
      "R-PW",
      "STR",
    ];

    const headers = [
      "Registers",
      "Description of risk or hazard",
      "Risk status",
      "Discipline",
      "Risk number",
      "Revision",
    ];

    const One2FiveList = ["1", "2", "3", "4", "5"];

    const { registers, loadRegisters } = Registers();

    loadRegisters(route.params.project_number);

    const reloadregisters = async () => {
      isLoading.value = true;
      await loadRegisters(route.params.project_number);
      isLoading.value = false;
    };

    const handleClickAdd = async () => {
      router.push({
        name: "Risk",
        params: {
          // to be diferent from a real risk number
          risk_number: "Add",
        },
      });
    };

    return {
      registers,
      route,
      reloadregisters,
      isLoading,
      disciplineList,
      One2FiveList,
      headers,
      handleClickAdd,
    };
  },
};
</script>

<style>
.register {
  background: #a6a7a6;
}
.register_table th {
  background-color: #a6a7a6;
  color: black;
}
</style>
