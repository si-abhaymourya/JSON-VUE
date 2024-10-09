

 <template>
  <div class="waf-body">
    <div class="table standings-table">
      <div class="table-head">
        <div class="table-row">
          <div class="table-data matches-position"><p class="text">#</p></div>
          <div class="table-data matches-team"><p class="text">Clubs</p></div>
          <div class="table-data matches-play"><p class="text">Played</p></div>
          <div class="table-data matches-won"><p class="text">Won</p></div>
          <div class="table-data matches-draw"><p class="text">Draw</p></div>
          <div class="table-data matches-lost"><p class="text">Lost</p></div>
          <div class="table-data matches-goal-for"><p class="text">GF</p></div>
          <div class="table-data matches-goal-against"><p class="text">GA</p></div>
          <div class="table-data matches-goal-difference"><p class="text">GD</p></div>
          <div class="table-data points"><p class="text">Points</p></div>
          <div class="table-data form-guide"><p class="text">Form</p></div>
        </div>
      </div>
      <div class="table-body">
        <div v-for="(team, index) in processedTeams" :key="index" class="table-row">
          <div class="waf-accordion-head">
            <div class="table-row table-row-head">
              <div class="table-data matches-position">
                <div class="table-data-wrap">
                  <p class="position">{{ index + 1 }}</p>
                </div>
              </div>
              <div class="table-data matches-team">
                <div class="table-data-wrap">
                  <div class="team-name">
                    <p class="name full-name">{{ team.team_name }}</p>
                    <p class="name short-name">{{ team.short_name }}</p>
                  </div>
                </div>
              </div>
              <div class="table-data matches-play"><p class="count">{{ team.played }}</p></div>
              <div class="table-data matches-won"><p class="count">{{ team.wins }}</p></div>
              <div class="table-data matches-draw"><p class="count">{{ team.draws }}</p></div>
              <div class="table-data matches-lost"><p class="count">{{ team.lost }}</p></div>
              <div class="table-data matches-goal-for"><p class="count">{{ team.gf }}</p></div>
              <div class="table-data matches-goal-against"><p class="count">{{ team.ga }}</p></div>
              <div class="table-data matches-goal-difference"><p class="count">{{ goalDifference(team.gf, team.ga) }}</p></div>
              <div class="table-data points"><p class="count">{{ team.points }}</p></div>
              <div class="table-data form-guide">
                <ul class="form-guide-listing">
                  <li v-for="(result, rIndex) in team.form" :key="rIndex" class="form-guide-item" 
                      :class="{
                        'form-win': result === 'W',
                        'form-loss': result === 'L',
                        'form-draw': result === 'D',
                        'form-no-result': result === null
                      }">
                    {{ result || '-' }}
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "../../public/css/standings.css";

export default {
  name: "StandingsTable",
  props: {
    teams: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      processedTeams: [],
    };
  },
  watch: {
    teams: {
      immediate: true,
      handler(newTeams) {
        this.processedTeams = this.processTeams(newTeams);
      },
    },
  },
  methods: {
    goalDifference(gf, ga) {
      return Number(gf) - Number(ga);
    },
    processTeams(teams) {
      return teams.map((team) => {
        const matchResults = team.match_result ? team.match_result.match : [];
        const form = matchResults.slice(0, 5).map(match => match.result);

        while (form.length < 5) {
          form.push(null);
        }

        return {
          team_name: team.team_name,
          short_name: team.short_name,
          played: team.played,
          wins: team.wins,
          draws: team.draws,
          lost: team.lost,
          gf: team.gf,
          ga: team.ga,
          points: team.points,
          form: form,
        };
      });
    },
  },
};
</script>

<style scoped>
.waf-body {
  width: 70%;
  margin: 0 auto;
}
.form-win {
  background-color: #00a83f;  
}
.form-loss {
  background-color: #d2001c;  
}
.form-draw, .form-no-result {
  background-color: #76766f;  
}
</style>
