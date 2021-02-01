<template>
  <data-view
    :title="title"
    :loaded="loaded"
    :date="date"
    :source-from="sourceFrom"
    :source-link="sourceLink"
    :title-id="titleId"
  >
    <template v-slot:button>
      <span />
    </template>
    <v-overlay absolute :value="!loaded" justify-center align-center>
      <scale-loader color="#1268d8" />
    </v-overlay>
    <v-layout :class="{ loading: !loaded }" column>
      <v-select
        v-model="search"
        :items = "regions"
        item-text="regionName"
        item-value="regionValue"
        label="居住地"
      ></v-select>
      <v-select
        v-model="search"
        :items = "annDate"
        label="公表日"
      ></v-select>
      <!-- disable-filteringを取りました -->
      <v-data-table
        :search="search"
        :headers="chartData.headers"
        :items="chartData.datasets"
        fixed-header
        height="375"
        :sort-by="sortBy"
        :sort-desc="sortDesc"
        :mobile-breakpoint="0"
        class="cardTable"
        :footer-props="{
          'items-per-page-options': [10, 20, 50, 100, -1],
          'items-per-page-text': $t('1ページ当たり')
        }"
      >
        <template slot="footer.page-text" slot-scope="props">
          {{
            $t('{itemsLength} 項目中 {pageStart} - {pageStop} ', {
              itemsLength: props.itemsLength,
              pageStart: props.pageStart,
              pageStop: props.pageStop
            })
          }}
        </template>
      </v-data-table>
    </v-layout>
    <template v-slot:infoPanel>
      <data-view-basic-info-panel
        :l-text="info ? info.lText : ''"
        :s-text="info ? info.sText : ''"
        :unit="info ? info.unit : ''"
      />
    </template>
  </data-view>
</template>

<style lang="scss">
.cardTable {
  &.v-data-table {
    th {
      padding: 8px 10px;
      height: auto;
      border-bottom: 1px solid $gray-4;
      white-space: nowrap;
      color: $gray-2;
      font-size: 12px;
    }
    tbody {
      tr {
        color: $gray-1;
        td {
          padding: 8px 10px;
          height: auto;
          font-size: 12px;
        }
        &:nth-child(odd) {
          td {
            background: rgba($gray-4, 0.3);
          }
        }
        &:not(:last-child) {
          td:not(.v-data-table__mobile-row) {
            border: none;
          }
        }
      }
    }
    .v-select {
      margin-left: 10px;
    }
  }
  .v-data-footer__pagination {
    margin-left: 0;
    margin-right: 5px;
  }
}

.loading {
  visibility: hidden;
}
</style>

<script>
import ScaleLoader from 'vue-spinner/src/ScaleLoader.vue'
const DataView = () => import('@/components/DataView.vue')
const DataViewBasicInfoPanel = () =>
  import('@/components/DataViewBasicInfoPanel.vue')

export default {
  data(){
    return{
      regions:[
        {regionName:'全て',regionValue:''},
        {regionName:'宗谷総合振興局管内',regionValue:'宗谷'},
        {regionName:'石狩振興局管内',regionValue:'石狩'},
        {regionName:'上川総合振興局管内',regionValue:'上川'},
        {regionName:'留萌振興局管内',regionValue:'留萌'},
        {regionName:'オホーツク総合振興局管内',regionValue:'オホーツク'},
        {regionName:'根室振興局管内',regionValue:'根室'},
        {regionName:'釧路総合振興局管内',regionValue:'釧路'},
        {regionName:'日高振興局管内',regionValue:'日高'},
        {regionName:'十勝総合振興局管内',regionValue:'十勝'},
        {regionName:'胆振総合振興局管内',regionValue:'胆振'},
        {regionName:'渡島総合振興局管内',regionValue:'渡島'},
        {regionName:'檜山振興局管内',regionValue:'檜山'},
        {regionName:'後志総合振興局管内',regionValue:'後志'},
        {regionName:'空知総合振興局管内',regionValue:'空知'},
      ]
    }
  },
  components: { DataView, DataViewBasicInfoPanel, ScaleLoader },
  props: {
    search:{
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    chartData: {
      type: Object,
      default: () => {}
    },
    date: {
      type: String,
      default: ''
    },
    info: {
      type: Object,
      required: false,
      default: () => {}
    },
    sourceFrom: {
      type: String,
      required: false,
      default: ''
    },
    sourceLink: {
      type: String,
      required: false,
      default: ''
    },
    loaded: {
      type: Boolean,
      required: true,
      default: false
    },
    sortBy: {
      type: String,
      required: false,
      default: ''
    },
    sortDesc: {
      type: Boolean,
      required: false,
      default: false
    },
    titleId: {
      type: String,
      required: false,
      default: ''
    },
    annDate:{
      type: Object,
      default: () => {}
    }
  }
}
</script>
