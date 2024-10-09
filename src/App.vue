<!-- <template>
  <div id="app">
    <AppNavbar />
    <StandingsTable :teams="teams" />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import StandingsTable from './components/standings.vue';
import AppNavbar from './components/NavbarComponent.vue'
export default {
  name: 'App',
  components: {
    StandingsTable,
    AppNavbar,
  },
  setup() {
  const teams = ref([]);

  const jsonDataParsing = async () => {
    const response = await fetch('/standings.json');
    if (response.ok) 
      return await response.json();
  };

  onMounted(async () => {
    const data = await jsonDataParsing();
    if (data) {
      teams.value = data.standings.groups[0].teams.team; 
    }
  });

  return {
    teams,
  };
},
}

</script>

 -->
 <template>
  <div id="app">
    <AppNavbar />
    <StandingsTable :teams="teams" />
  </div>
</template>

<script>
import StandingsTable from './components/standings.vue';
import AppNavbar from './components/NavbarComponent.vue';

export default {
  name: 'App',
  components: {
    StandingsTable,
    AppNavbar,
  },
  data() {
    return {
      teams: [],
    };
  },
  async mounted() {
    const data = await this.jsonDataParsing();
    if (data) {
      this.teams = data.standings.groups[0].teams.team; 
    }
  },
  methods: {
    async jsonDataParsing() {
      const response = await fetch('/standings.json');
      if (response.ok) 
        return await response.json();
    },
  },
};
</script>
