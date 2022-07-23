<template>
  <Header :header="this.header" />
  <div class="content-container">
    <MissionView
      :missions="this.missions"
      :missionMarkdown="this.current_md"
      :selected="this.mission_slug"
      v-on:missionSelected="handleMissionSelected"
    />

    <EventsView :events="this.events" />
    <section class="section-container" id="main-tab">
      <div class="main-tab-header" style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img :src="mainTabIcon" />
          <h1>{{ mainTabTitle }}</h1>
        </div>
        <TabButton
          v-for="item in this.options.panelOptions"
          :name="item"
          :hidden="item === this.options.mainPanel"
          :key="item"
          @click="selectMainPanel(item)"
        />
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <transition name="fade" mode="out-in">
          <PilotsView v-if="this.options.mainPanel === 'pilot'" :pilots="this.pilots" />
          <NPCView v-else-if="this.options.mainPanel === 'npc'" :npcs="this.npcs" />
          <GlossaryView v-else-if="this.options.mainPanel === 'glossary'" />
          <ClocksView v-else-if="this.options.mainPanel === 'clock'" :clocks="this.clocks" />
        </transition>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer />
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import MissionView from './components/layout/MissionView.vue';
import EventsView from './components/layout/EventsView.vue';
import PilotsView from './components/layout/PilotsView.vue';
import NPCView from './components/layout/NPCView.vue';
import ClocksView from './components/layout/ClocksView.vue';
import GlossaryView from './components/layout/GlossaryView.vue';
import TabButton from './components/TabButton.vue'

export default {
  components: {
    Header,
    Footer,
    MissionView,
    EventsView,
    PilotsView,
    NPCView,
    GlossaryView,
    ClocksView,
    TabButton,
  },

  data() {
    return {
      "mission_slug": "006",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001",
          "name": "Bug-Hunt",
          "status": "success"
        },
		{
          "slug": "002",
          "name": "Vigilant Gaze",
          "status": "partial-success"
        },
		{
          "slug": "003",
          "name": "Floodgate",
          "status": "success"
        },
		{
          "slug": "004",
          "name": "Rallying Cry",
          "status": "success"
        },
		{
          "slug": "005",
          "name": "Split-Knuckle Haymaker",
          "status": "success"
        },
		{
			"slug": "006",
			"name": "Dawn of a New War",
			"status": "start"
		}
      ],
      "pilots": [
        {
          "callsign": "Dwarf",
          "alias": "Arthur Fredrick Bradshaw",
          "code": "Bradshaw.Arthur.Fredrick:5515314a-b2e8-4717-a7ef-853bf036de35//NDL-C-BARYON-TEMPLE",
          "corpro": "IPS-N",
          "frame": "Blackbeard",
          "mech": "Atgeir"
        },
        {
          "callsign": "Miracle",
          "alias": "Miroslava Vlablinskaja",
          "code": "Vlablinskaja.Miroslava:587f9a36-bf6b-4505-bc21-1b88e18a144d//NDL-C-SOLAR-VECTOR",
          "corpro": "HA",
          "frame": "Barbarossa",
          "mech": "Perun"
        },
        {
          "callsign": "Eclipse",
          "alias": "ᏃᏈᏏ ᎤᏕᏅ",
          "code": "ᎤᏕᏅ.Cat.Noquisi.Udenv./.ᏃᏈᏏ:602cf4a4-58a4-4c42-a736-8f64c7b0b75a//NDL-C-SINGULARITY-HAMMER",
          "corpro": "HORUS",
          "frame": "Balor",
          "mech": "Phobos"
        },
        {
          "callsign": "Shrike",
          "alias": "Sokolova",
          "code": "Sokolova:9b84f5f3-3a22-44c9-853d-c223fe3dca03//NDL-C-BRONZE-SWORD",
          "corpro": "IPS-N",
          "frame": "Raleigh",
          "mech": "Creosote"
        },
        {
          "callsign": "Rhapsody",
          "alias": "Caelia Pagett",
          "code": "Pagett.Caelia:a222e882-b71c-418c-ad8a-2b0dcb7b5b5d//NDL-C-LUNAR-DREAM",
          "corpro": "SSC",
          "frame": "Dusk Wing",
          "mech": "Staccato"
        },
		{
          "callsign": "Violet",
          "alias": "Fjola Skuldari",
          "code": "Skuldari.Fjola:f570e86b-d033-4255-9a85-f14cacaa2c59//NDL-C-DISCORDANT-VECTOR",
          "corpro": "SSC",
          "frame": "Monarch",
          "mech": "Diluvium"
        }
      ],
      "npcs": [
        {
          "name": "Brava Hadura",
          "affiliation": "Evergreen",
          "pronouns": "She/Her",
          "notes": "Militia Commander"
        },
		{
          "name": "Castor Fielding",
          "affiliation": "Evergreen",
          "pronouns": "He/Him",
          "notes": "Chief Engineer"
        },
		{
          "name": "Dthall Ordo",
          "affiliation": "Hercynian United Cities",
          "pronouns": "She/Her",
          "notes": "HUC Ranger Liaison"
        },
		{
          "name": "Endeavor",
          "affiliation": "Hercynian United Cities",
          "pronouns": "They/Them",
          "notes": "Egregorian Overmind"
        },
		{
          "name": "Ilyr Ordo",
          "affiliation": "Hercynian United Cities",
          "pronouns": "He/Him",
          "notes": "Hivehome Commander"
        },
		{
          "name": "Primoz Commorand",
          "affiliation": "Hercynian United Cities",
          "pronouns": "He/Him",
          "notes": "Daylight Commnader"
        },
		{
          "name": "Pyotr Heidel",
          "affiliation": "Hercynian United Cities",
          "pronouns": "He/Him",
          "notes": "Mycol Fields Commander"
        },
		{
          "name": "Terror",
          "affiliation": "Hercynian United Cities",
          "pronouns": "They/Them",
          "notes": "HUC Ranger Commander"
        }
      ],
      "header": {
        "planet": "Hercynia",
        "year": "5014u",
        "system": "Ardennes-3",
        "gate": "Atlas-Quanokrim",
        "ring": "Atlas-Line",
        "headerTitle": "Hercynian",
        "headerSubtitle": "United Cities",
        "subheaderTitle": "Rangers",
        "subheaderSubtitle": "382nd MECH",
      },
      "clocks": [
      ],
      "options": {
        "eventsMarkdownPerMission": true,
        "mainPanel": "pilot",
        "panelOptions": [
          "pilot",
          "npc",
          "glossary",
          "clock"
        ]
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {
    mainTabTitle() {
      if (this.options.mainPanel === "pilot") return "Pilot Roster"
      if (this.options.mainPanel === "npc") return "Persons Registry"
      if (this.options.mainPanel === "glossary") return "Lexicon"
      if (this.options.mainPanel === "clock") return "Clocks"
    },
    mainTabIcon() {
      return `/icons/${this.options.mainPanel}-icon.svg`
    }
  },

  methods: {
    handleMissionSelected(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if (this.options.eventsMarkdownPerMission) {
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if (self.options.eventsMarkdownPerMission) {
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    },
    selectMainPanel(panel) {
      this.options.mainPanel = panel;
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>
