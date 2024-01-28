<script setup>
const samples = [
  { id: "1", name: "Zahnreinigung (Prophylaxe)" },
  { id: "2", name: "Füllung: Composite-Füllungen" },
  { id: "3", name: "Füllung: Amalgam-Füllungen" },
  { id: "4", name: "Füllung: Gold-Inlays" },
  { id: "5", name: "Füllung: Keramik-Inlays" },
  { id: "6", name: "Füllung: Kunststoff-Inlays" },
  { id: "7", name: "Wurzelkanalbehandlung (Endodontie)" },
  { id: "8", name: "Zahnextraktion" },
  { id: "9", name: "Zahnimplantate" },
  { id: "10", name: "Zahnspangen (Kieferorthopädie)" },
  { id: "12", name: "Kronen und Brücken" },
  { id: "13", name: "Zahnfleischbehandlung (Parodontologie)" },
  { id: "14", name: "Zahnprothesen (Teil- und Vollprothesen)" },
  { id: "15", name: "Weisheitszahnentfernung" },
  { id: "16", name: "Inlays und Onlays" },
  { id: "17", name: "Zahnbleaching" },
  { id: "18", name: "Zahnaufbau" },
  { id: "19", name: "Kiefergelenkbehandlungen" },
  { id: "20", name: "Zahnregulierung bei Kindern" },
  { id: "21", name: "Zahnfissurenversiegelung" },
  { id: "22", name: "Mundhygieneberatung" },
  { id: "23", name: "Zahnfleischtransplantation" },
  { id: "24", name: "Prothetische Versorgungen" },
  { id: "25", name: "Notfallbehandlungen" },
  { id: "26", name: "Zahnspangen-Nachjustierung" },
  { id: "27", name: "Plaqueentfernung" },
  { id: "28", name: "Interdentale Reinigung" },
  { id: "29", name: "Zahnsteinentfernung" },
  { id: "30", name: "Kieferorthopädische Retention" },
  { id: "31", name: "Zahnwurzelbehandlung" },
  { id: "32", name: "Zahnkorrekturen" },
  { id: "33", name: "Dentale Röntgenaufnahmen" },
  { id: "34", name: "Zahnkrone-Erneuerung" },
  { id: "35", name: "Zahnfleischabszess-Behandlung" },
];

const sections = [
  {
    name: "Anamnese",
    positions: [
      "9100 - Anamnese / Befundaufnahme neuer Patient",
      "9101 - Anamnese / Befundaufnahme Recall-Patient",
    ],
  },
  {
    name: "Mundhygiene",
    positions: [
      "9102 - Mundhygiene Instruktion / Information / Motivation",
      "9103 - Parodontalstatus",
      "9104 - Hygiene- und Entzündungsstatus",
      "9105 - Plaqueanfärbung",
      "9106 - Fluoridierung",
      "9107 - Lokale Desinfektion",
      "9110 - Zahnmedizinische Ernährungsberatung",
      "9111 - Raucherintervention",
      "9112 - Instruktion Drittpersonen",
      "9113 - Erosionsberatung",
      "9114 - Gruppenprophylaxe / Öffentlichkeitsarbeit",
    ],
  },
  {
    name: "Füllungen",
    positions: [
      "9208 - Vitalitätstest",
      "9209 - Desensibilisierung",
      "9210 - Fluoridierung",
      "9211 - Überschussentfernung / Füllungspolitur / Rekonturieren",
      "9212 - Provisorische Füllung",
      "9213 - Krone provisorisch rezementieren",
      "9214 - Kofferdam",
      "9215 - Fissuren-Versiegelung",
      "9216 - Abformung",
    ],
  },
  {
    name: "Bleaching",
    positions: [
      "9220 - Home Bleaching inkl. Abdrücke",
      "9221 - In Office Bleaching",
      "9223 - Bleaching Nachkontrolle",
      "9224 - Zahnschmuck",
    ],
  },
  {
    name: "Prothetik",
    positions: ["9225 - Prothesenpflege", "9226 - Schienenpflege"],
  },
  {
    name: "Versäumte Sitzung",
    positions: ["9300 - Versäumte Sitzung", "9301 - Praxisexterne Behandlung"],
  },
];

const selectedId = ref(null);
const selectedPositions = ref([]);
const positionFocusIdx = ref(-1);

const reset = () => {
  selectedId.value = null;
};

const addPositions = (positions) => {
  selectedPositions.value.push(
    ...positions.map((p) => ({ name: p, deleted: false }))
  );
};

const numKeys = ["1", "2", "3", "4", "5", "6", "7", "8", "9"];
const log = (e) => {
  const key = e.key;

  if (numKeys.includes(key)) {
    const idx = parseInt(key) - 1;

    if (selectedId.value == null) {
      selectedId.value = samples[idx].id;
    } else {
      addPositions(sections[idx].positions);
    }
  } else if (key === "Enter") {
    reset();
  } else if (key == "ArrowDown") {
    positionFocusIdx.value = Math.min(
      positionFocusIdx.value + 1,
      selectedPositions.value.length - 1
    );
  } else if (key == "ArrowUp") {
    positionFocusIdx.value = Math.max(positionFocusIdx.value - 1, 0);
  } else if (key == "ArrowLeft" || key == "ArrowRight" || e.code == "Space") {
    selectedPositions.value[positionFocusIdx.value].deleted =
      !selectedPositions.value[positionFocusIdx.value].deleted;
  }
  console.log(e);
};
</script>
<template>
  <div
    class="grid grid-cols-2 gap-3 w-full p-3 select-none h-screen"
    @click.right.prevent="reset()"
    @keydown="log"
    tabindex="0"
    autofocus
  >
    <div>
      <div class="inline-grid grid-cols-3 gap-3">
        <a
          class="bg-green-300 uppercase flex items-center justify-center rounded cursor-pointer hover:bg-green-200 h-20"
          v-if="selectedId"
          @click.stop="reset()"
          >Zurück</a
        >
        <!-- Samples -->
        <a
          v-if="selectedId == null"
          v-for="(sample, idx) in samples"
          :key="sample.id"
          class="bg-gray-200 p-3 rounded cursor-pointer hover:bg-gray-300 h-20"
          @click.stop="selectedId = sample.id"
        >
          <span
            v-if="idx < 9"
            class="bg-gray-600 text-gray-100 inline-block px-2 py-1 text-xs rounded"
            >{{ idx + 1 }}</span
          >
          {{ sample.name }}
        </a>
        <!-- Sections of Samples -->
        <a
          v-else
          v-for="(section, idx) in sections"
          :key="section.name"
          class="bg-gray-200 p-3 rounded cursor-pointer hover:bg-gray-300 h-full h-20"
          @click="addPositions(section.positions)"
        >
          <span
            v-if="idx < 9"
            class="bg-gray-600 text-gray-100 inline-block px-2 py-1 text-xs rounded"
            >{{ idx + 1 }}</span
          >
          {{ section.name }}
        </a>
      </div>
    </div>
    <div>
      <!-- Selected positions -->
      <div class="flex flex-col gap-3 rounded-lg border border-gray-300 p-3">
        <div
          v-for="(position, idx) in selectedPositions"
          :key="position.name"
          class="bg-gray-200 p-3 rounded cursor-pointer hover:bg-gray-300"
          :class="{
            'opacity-30': position.deleted,
            'border border-gray-600': idx == positionFocusIdx,
          }"
          @click="position.deleted = !position.deleted"
        >
          {{ position.name }}
        </div>
      </div>
    </div>
  </div>
</template>
