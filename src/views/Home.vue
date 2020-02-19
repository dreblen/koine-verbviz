<template>
  <v-container>
    <v-app-bar app elevate-on-scroll height="300">
      <v-container>
        <v-row>
          <v-col class="display-1 text-center">{{ gloss.greek }}</v-col>
        </v-row>
        <v-row class="mb-5">
          <v-col class="headline text-center">{{ gloss.english }}</v-col>
        </v-row>
        <v-row>
        </v-row>
        <v-row align="center" justify="center" class="mb-5">
          <v-btn-toggle mandatory v-model="voice">
            <v-btn v-for="voice in voices" :key="voice">{{ voice }}</v-btn>
          </v-btn-toggle>
        </v-row>
        <v-row>
          <v-col v-for="mood in allowedMoods" :key="mood" class="text-center">{{ mood }}</v-col>
        </v-row>
      </v-container>
    </v-app-bar>
    <v-row>
      <v-col>
        <v-item-group mandatory>
          <v-card v-for="tense in tenses" :key="tense" class="mb-2">
            <v-card-text class="text-center">
              {{ tense }}
              <v-row>
                <v-col v-for="mood in allowedMoods" :key="mood">
                  <!--
                  We want the column here no matter what for spacing, but only create an
                  item if it's a valid combination for this tense/mood
                  -->
                  <v-item v-if="glosses[mood][tense]" v-slot:default="{ active, toggle }">
                    <v-btn
                      block
                      :large="true"
                      :color="(active) ? 'primary' : 'secondary'"
                      @click="() => { toggle(); setActive(mood, tense); }"
                    />
                  </v-item>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-item-group>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'home',
  data: () => ({
    // Tense
    tenses: [
      'Aorist',
      'Imperfect',
      'Perfect',
      'Present',
      'Future'
    ],
    tense: 'Aorist',

    // Voice
    voices: [
      'Active',
      'Middle',
      'Passive'
    ],
    voice: 0,

    // Mood
    moods: [
      'Indicative',
      'Subjunctive',
      'Imperative'
    ],
    mood: 'Indicative',

    // Glosses/mappings
    glosses: {
      Indicative: {
        Aorist: {
          Active: { greek: 'ελυσα', english: 'I loosed' },
          Middle: { greek: 'ελυσαμην', english: 'I loosed for myself' },
          Passive: { greek: 'ελυθην', english: 'I was loosed' }
        },
        Imperfect: {
          Active: { greek: 'ελυον', english: 'I was loosing' },
          Middle: { greek: 'ελυομην', english: 'I was loosing for myself' },
          Passive: { greek: 'ελυομην', english: 'I was being loosed' }
        },
        Perfect: {
          Active: { greek: 'λελυκα', english: 'I have loosed' },
          Middle: { greek: 'λελυμαι', english: 'I have loosed for myself' },
          Passive: { greek: 'λελυμαι', english: 'I have been loosed' }
        },
        Present: {
          Active: { greek: 'λυω', english: 'I am loosing' },
          Middle: { greek: 'λυομαι', english: 'I am loosing for myself' },
          Passive: { greek: 'λυομαι', english: 'I am being loosed' }
        },
        Future: {
          Active: { greek: 'λυσω', english: 'I will loose' },
          Middle: { greek: 'λυσομαι', english: 'I will loose for myself' },
          Passive: { greek: 'λυθησομαι', english: 'I will be loosed' }
        }
      },
      Subjunctive: {
        Aorist: {
          Active: { greek: 'λυσω', english: 'I may loose' },
          Middle: { greek: 'λυσωμαι', english: 'I may loose for myself' },
          Passive: { greek: 'λυθω', english: 'I may be loosed' }
        },
        Present: {
          Active: { greek: 'λυω', english: 'I may be loosing' },
          Middle: { greek: 'λυωμαι', english: 'I may be loosing for myself' },
          Passive: { greek: 'λυωμαι', english: 'I may be being loosed' }
        }
      },
      Imperative: {
        Aorist: {
          Active: { greek: 'λυσον', english: 'Loose!' },
          Middle: { greek: 'λυσαι', english: 'Loose for yourself!' },
          Passive: { greek: 'λυθητι', english: 'Be loosed!' }
        },
        // NOTE: Technically possible, but not common enough to warrant inclusion.
        // Perfect: {
        //   Active: { greek: 'λελυκε', english: '' },
        //   Middle: { greek: 'λελυσο', english: '' },
        //   Passive: { greek: 'λελυσο', english: '' }
        // },
        Present: {
          Active: { greek: 'λυε', english: 'Be loosing!' },
          Middle: { greek: 'λυου', english: 'Be loosing for yourself!' },
          Passive: { greek: 'λυου', english: 'Be being loosed!' }
        }
      }
    }
  }),
  computed: {
    allowedMoods: function () {
      // If no comma-separated list of moods was given in the query string, then
      // return the regular list
      if (!this.$route.query.moods) {
        return this.moods
      } else {
        // Make sure any values we were given are valid
        let newMoods = this.$route.query.moods.split(',').filter((v) => {
          return this.moods.indexOf(v) !== -1
        })

        // If none of the moods we were given were valid, return the regular list
        if (newMoods.length === 0) {
          return this.moods
        } else {
          return newMoods
        }
      }
    },
    gloss: function () {
      return this.glosses[this.mood][this.tense][this.voices[this.voice]]
    }
  },
  methods: {
    setActive: function (mood, tense) {
      this.mood = mood
      this.tense = tense
    }
  }
}
</script>
