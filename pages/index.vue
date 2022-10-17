<template>
  <v-row justify="center" align="center">
    <v-col class="text-center">
      <v-expansion-panels focusable>
        <v-expansion-panel>
          <v-expansion-panel-header class="text-h6 py-3">
            Фильтрация
          </v-expansion-panel-header>
          <v-expansion-panel-content class="my-1">
            <v-form ref="form">
            <v-row justify="center" align="center" class="pt-4">
              <v-col style="display: contents" class="text-h4">1</v-col>
              <v-col>
                <v-select
                  v-model="selectedColumn"
                  :items="columns"
                  label="Выбор колонки"
                  outlined
                  hide-details
                />
              </v-col>
              <v-col style="display: contents" class="text-h4">2</v-col>
              <v-col>
                <v-select
                  v-model="selectedCondition"
                  :items="conditions"
                  label="Выбор условия"
                  outlined
                  hide-details
                  :disabled="!selectedColumn"
                />
              </v-col>
              <v-col style="display: contents" class="text-h4">3</v-col>
              <v-col>
                <v-text-field
                  v-model="filterValue"
                  label="Значение для фильтрации"
                  outlined
                  clearable
                  hide-details
                  :disabled="!selectedColumn"
                />
              </v-col>
                <v-btn depressed color="primary" class="mr-2 py-7" @click="goFilter">
                  Фильтровать
                </v-btn>
                <v-btn depressed color="primary" class="py-7" @click="resetFilter">
                  Сбросить
                </v-btn>
            </v-row>
          </v-form>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
      <v-simple-table class="mt-6">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left text-body-1 font-weight-bold">#</th>
              <th class="text-left text-body-1 font-weight-bold">Дата</th>
              <th class="text-left text-body-1 font-weight-bold">Название<v-btn class="ml-1" icon color="grey" @click="sortName"><v-icon>{{ nameIcon }}</v-icon></v-btn></th>
              <th class="text-left text-body-1 font-weight-bold">Количество<v-btn class="ml-1" icon color="grey" @click="sortAmount"><v-icon>{{ amountIcon }}</v-icon></v-btn></th>
              <th class="text-left text-body-1 font-weight-bold">Расстояние<v-btn class="ml-1" icon color="grey" @click="sortDistance"><v-icon>{{ distanceIcon }}</v-icon></v-btn></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in items" :key="item.index">
              <td class="text-left pl-2">{{ item.index + 1 }}</td>
              <td class="text-left pl-2">{{ item.data }}</td>
              <td class="text-left pl-2">{{ item.name }}</td>
              <td class="text-left pl-2">{{ item.amount }}</td>
              <td class="text-left pl-2">{{ item.distance }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      <v-pagination v-if="sourceArray.length > 10" v-model="page" :length="5" class="my-4" />
    </v-col>
  </v-row>
</template>

<script>
import * as data from '~/static/source.json'
export default {
  name: 'IndexPage',
  data () {
    return {
      sourceArray: [],
      page: 1,
      selectedColumn: '',
      columns: ['Дата', 'Название', 'Количество', 'Расстояние'],
      selectedCondition: '',
      filterValue: '',
      nameIcon: 'mdi-sort-reverse-variant',
      amountIcon: 'mdi-sort-reverse-variant',
      distanceIcon: 'mdi-sort-reverse-variant',
      nameFlag: 0,
      amountFlag: 0,
      distanceFlag: 0
    }
  },
  mounted () {
    this.sourceArray = data.items
  },
  computed: {
    items () {
      if (this.page === 1) { return this.sourceArray.slice(0, 10) }
      else if (this.page === 2) { return this.sourceArray.slice(10, 20) }
      else if (this.page === 3) { return this.sourceArray.slice(20, 30) }
      else if (this.page === 4) { return this.sourceArray.slice(30, 40) }
      else { return this.sourceArray.slice(40, 50) }
    },
    conditions () {
      if (this.selectedColumn !== 'Название') {
        return ['Равно', 'Больше', 'Меньше']
      } else {
        return ['Равно', 'Содержит', 'Больше', 'Меньше']
      }
    }
  },
  methods: {
      resetFilter () {
        this.$refs.form.reset()
        this.sourceArray = data.items
      },
      goFilter () {
        if (this.selectedColumn === 'Дата') {
          if (this.selectedCondition === 'Равно') {
            this.sourceArray = this.sourceArray.filter(item => new Date(item.data) === new Date(this.filterValue))
          } else if (this.selectedCondition === 'Больше') {
            this.sourceArray = this.sourceArray.filter(item => new Date(item.data) > new Date(this.filterValue))
          } else if (this.selectedCondition === 'Меньше') {
            this.sourceArray = this.sourceArray.filter(item => new Date(item.data) < new Date(this.filterValue))
          }
        }
        if (this.selectedColumn === 'Название') {
          if (this.selectedCondition === 'Равно') {
            this.sourceArray = this.sourceArray.filter(item => item.name === this.filterValue)
          } else if (this.selectedCondition === 'Содержит') {
            this.sourceArray = this.sourceArray.filter(item => item.name.includes(this.filterValue))
          } else if (this.selectedCondition === 'Больше') {
            this.sourceArray = this.sourceArray.filter(item => item.name > this.filterValue)
          } else if (this.selectedCondition === 'Меньше') {
            this.sourceArray = this.sourceArray.filter(item => item.name < this.filterValue)
          }
        }
        if (this.selectedColumn === 'Количество') {
          if (this.selectedCondition === 'Равно') {
            this.sourceArray = this.sourceArray.filter(item => item.amount === this.filterValue)
          } else if (this.selectedCondition === 'Больше') {
            this.sourceArray = this.sourceArray.filter(item => item.amount > this.filterValue)
          } else if (this.selectedCondition === 'Меньше') {
            this.sourceArray = this.sourceArray.filter(item => item.amount < this.filterValue)
          }
        }
        if (this.selectedColumn === 'Расстояние') {
          if (this.selectedCondition === 'Равно') {
            this.sourceArray = this.sourceArray.filter(item => item.distance === this.filterValue)
          } else if (this.selectedCondition === 'Больше') {
            this.sourceArray = this.sourceArray.filter(item => item.distance > this.filterValue)
          } else if (this.selectedCondition === 'Меньше') {
            this.sourceArray = this.sourceArray.filter(item => item.distance < this.filterValue)
          }
        }
      },
      sortName () {
        this.amountFlag = this.distanceFlag = 0
        this.amountIcon = this.distanceIcon = 'mdi-sort-reverse-variant'
        ++this.nameFlag
        if (this.nameFlag > 2) {this.nameFlag = 0}
        if (this.nameFlag === 0) {
          this.nameIcon = 'mdi-sort-reverse-variant'
          this.sourceArray.sort((a, b) => a.index > b.index ? 1 : -1)
        } else if (this.nameFlag === 1) {
          this.nameIcon = 'mdi-arrow-down'
          this.sourceArray.sort((a, b) => a.name > b.name ? 1 : -1)
        } else {
          this.nameIcon = 'mdi-arrow-up'
          this.sourceArray.sort((a, b) => a.name < b.name ? 1 : -1)
        }
      },
      sortAmount () {
        this.nameFlag = this.distanceFlag = 0
        this.nameIcon = this.distanceIcon = 'mdi-sort-reverse-variant'
        ++this.amountFlag
        if (this.amountFlag > 2) {this.amountFlag = 0}
        if (this.amountFlag === 0) {
          this.amountIcon = 'mdi-sort-reverse-variant'
          this.sourceArray.sort((a, b) => a.index > b.index ? 1 : -1)
        } else if (this.amountFlag === 1) {
          this.amountIcon = 'mdi-arrow-down'
          this.sourceArray.sort((a, b) => a.amount > b.amount ? 1 : -1)
        } else {
          this.amountIcon = 'mdi-arrow-up'
          this.sourceArray.sort((a, b) => a.amount < b.amount ? 1 : -1)
        }
      },
      sortDistance () {
        this.amountFlag = this.nameFlag = 0
        this.amountIcon = this.nameIcon = 'mdi-sort-reverse-variant'
        ++this.distanceFlag
        if (this.distanceFlag > 2) {this.distanceFlag = 0}
        if (this.distanceFlag === 0) {
          this.distanceIcon = 'mdi-sort-reverse-variant'
          this.sourceArray.sort((a, b) => a.index > b.index ? 1 : -1)
        } else if (this.distanceFlag === 1) {
          this.distanceIcon = 'mdi-arrow-down'
          this.sourceArray.sort((a, b) => a.distance > b.distance ? 1 : -1)
        } else {
          this.distanceIcon = 'mdi-arrow-up'
          this.sourceArray.sort((a, b) => a.distance < b.distance ? 1 : -1)
        }
      }
    }
}
</script>
